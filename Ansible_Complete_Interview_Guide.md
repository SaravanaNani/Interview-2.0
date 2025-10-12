
# ⚙️ Ansible Complete Interview Guide (Basic + Intermediate + Project Integration)

This document combines **Ansible fundamentals**, **intermediate use cases**, and **real DevOps project integration** examples — especially how you used **Terraform + Ansible + Jenkins** for automation.

---

## 🧩 1. What is Ansible and Why It’s Used in DevOps

**Ansible** is an open-source automation tool used for **configuration management**, **application deployment**, and **infrastructure orchestration**.

It connects to servers via **SSH** (no agents required) and executes **YAML-based playbooks** to ensure consistent configurations.

**Example (Project Use):**
> Used Ansible to configure Java, Python, and Tomcat on GCP instances created by Terraform. Automated WAR file deployment to Tomcat via Jenkins.

**Key Benefits:**
- Agentless
- Idempotent (no repeated changes)
- YAML syntax for easy readability

**Command Example:**
```bash
ansible all -m ping -i inventory.ini
```

---

## 🧩 2. Difference Between `ansible` and `ansible-playbook`

| Command | Purpose | Example |
|----------|----------|----------|
| `ansible` | Runs one-off ad-hoc commands | `ansible all -m ping` |
| `ansible-playbook` | Executes YAML playbooks (multi-step) | `ansible-playbook setup.yml` |

**Example:**
```bash
ansible web -m yum -a "name=httpd state=present"
ansible-playbook deploy.yml
```

---

## 🧩 3. Passing Variables in Ansible

| Method | Description | Example |
|---------|--------------|----------|
| Playbook Vars | Defined inside YAML | `vars: { package: httpd }` |
| Inventory Vars | Next to host | `web ansible_host=10.0.0.1 port=8080` |
| CLI | Passed at runtime | `ansible-playbook site.yml -e "version=1.2.2"` |
| Vars File | Stored separately | `vars_files: - vars/main.yml` |
| Lookup Plugin | Fetch data dynamically | `{{ lookup('file', '/path/data.txt') }}` |

**Command Example:**
```bash
ansible-playbook site.yml -e "env=prod version=2.0"
```

---

## 🧩 4. Running Playbook on Subset of Hosts

Use `-l` flag to limit hosts.

**Example:**
```bash
ansible-playbook deploy.yml -l web
ansible-playbook deploy.yml -l web1
```

**Inventory Example:**
```ini
[web]
web1 ansible_host=10.0.0.1
web2 ansible_host=10.0.0.2
```

---

## 🧩 5. Roles in Ansible

> **Roles** are structured ways to organize playbooks into reusable, modular components.

**Role Structure:**
```
roles/
  tomcat/
    tasks/
    handlers/
    templates/
    files/
    vars/
```

**Main Playbook:**
```yaml
- hosts: web
  roles:
    - jdk_install
    - tomcat_config
    - deploy_app
```

**Advantages:**
- Modular and reusable  
- Cleaner code organization  
- Ideal for large environments

---

## 🧩 6. Including Playbooks

Include one playbook inside another using:
- `import_playbook:` → Static include
- `include:` → Dynamic include

**Example:**
```yaml
- import_playbook: terraform-apply.yml
- import_playbook: ansible-configure.yml
```

---

## 🧩 7. Tags and Conditionals

**Tags** help run or skip specific tasks.  
**Conditionals** (`when:`) run tasks based on conditions.

```yaml
- name: Install Tomcat
  yum:
    name: tomcat
    state: present
  tags:
    - install

- name: Start Tomcat
  service:
    name: tomcat
    state: started
  when: ansible_os_family == "RedHat"
  tags:
    - start
```

**Commands:**
```bash
ansible-playbook app.yml --tags "install"
ansible-playbook app.yml --skip-tags "start"
```

---

## 🧩 8. Handlers and Notifications

Handlers are triggered when a task reports a change.

**Example:**
```yaml
- name: Copy Tomcat config
  template:
    src: server.xml.j2
    dest: /usr/local/tomcat/conf/server.xml
  notify:
    - restart tomcat

handlers:
  - name: restart tomcat
    service:
      name: tomcat
      state: restarted
```

---

## 🧩 9. Ansible Vault (Encrypt Sensitive Data)

Used to encrypt passwords, keys, and tokens.

**Commands:**
```bash
ansible-vault create secrets.yml
ansible-vault encrypt creds.yml
ansible-vault decrypt creds.yml
ansible-playbook site.yml --ask-vault-pass
```

**Example Use:**
```yaml
vars_files:
  - vault.yml
```

---

## 🧩 10. Dynamic Inventory

Fetches host data from cloud providers dynamically.

**GCP Example:**
```yaml
plugin: gcp_compute
projects:
  - my-gcp-project
auth_kind: serviceaccount
service_account_file: /etc/ansible/gcp-key.json
```

**Command:**
```bash
ansible-inventory -i gcp_compute.yml --list
```

---

## 🧩 11. Terraform + Ansible Integration

**Flow:**
1. Terraform creates infrastructure.
2. Terraform outputs instance IPs.
3. Ansible uses these IPs to configure servers.

**Terraform Example:**
```hcl
output "ansible_inventory" {
  value = join("\n", formatlist("%s ansible_host=%s", google_compute_instance.web.*.name, google_compute_instance.web.*.network_interface[0].access_config[0].nat_ip))
}
```

**Execution:**
```bash
terraform output ansible_inventory > inventory.ini
ansible-playbook -i inventory.ini site.yml
```

**Your Project Workflow:**
- Terraform → Provision infra  
- Jenkins → Parameterized control (apply/destroy)  
- Ansible → Configure servers + Deploy apps  

---

## 🧩 12. Jenkins + Ansible Integration

**Jenkinsfile Example:**
```groovy
pipeline {
  agent any
  parameters {
    choice(name: 'ACTION', choices: ['apply', 'destroy', 'skip'])
  }
  stages {
    stage('Terraform') {
      steps {
        script {
          if (params.ACTION == 'apply') {
            sh 'ansible-playbook apply.yml --tags "terraform"'
          } else if (params.ACTION == 'destroy') {
            sh 'ansible-playbook destroy.yml --tags "terraform"'
          } else {
            echo 'Skipping Terraform...'
          }
        }
      }
    }
    stage('Configuration') {
      steps {
        sh 'ansible-playbook configure.yml --tags "ansible"'
      }
    }
  }
}
```

---

## 🧩 13. Example: Ansible Role for Tomcat Setup

**Tasks (Tomcat Role):**
```yaml
- name: Install Tomcat
  unarchive:
    src: apache-tomcat-9.tar.gz
    dest: /opt/
    remote_src: yes

- name: Start Tomcat
  shell: nohup /opt/apache-tomcat-9/bin/startup.sh &
```

**Playbook:**
```yaml
- hosts: web
  roles:
    - tomcat_setup
    - deploy_app
```

---

## 🧩 14. Debugging and Troubleshooting

**Commands:**
```bash
ansible-playbook site.yml -vvv
ansible-playbook site.yml --check
ansible -m ping all
```

**Common Issues:**
- SSH connectivity failure  
- Undefined variables  
- YAML indentation issues  
- Permission denied errors  

**Tip:** Always test with `--check` before applying changes.

---

## 🧠 Quick Revision Summary

| Concept | Description |
|----------|--------------|
| **Tags** | Run or skip specific tasks |
| **Handlers** | Restart/reload services when changed |
| **Vault** | Encrypt sensitive files |
| **Dynamic Inventory** | Fetch host info from cloud |
| **Terraform + Ansible** | Infra + Config automation |
| **Jenkins + Ansible** | CI/CD automation |
| **Roles** | Modular playbook organization |
| **Debugging** | Use `-vvv`, `--check`, `ping` |

---

**Prepared by:** Saravana L  
*(DevOps Engineer | Terraform | Ansible | Jenkins | GCP | EKS)*

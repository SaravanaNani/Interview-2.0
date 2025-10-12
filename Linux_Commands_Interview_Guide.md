
# 🐧 Linux Commands – Interview Guide (Simple & Practical for 2 Years Exp)

This guide covers the **most-used Linux commands** you’ll need in DevOps interviews and day-to-day work. Each topic has:
- **What it does**
- **Command(s) to use**
- **Small example you can try**

---

## 1) Files & Navigation

**What:** Move around, list files, see hidden files, view paths.  
**Commands:**
```bash
pwd                    # show current directory
ls -la                 # list all files (incl. hidden) with details
cd /var/log            # change directory
mkdir -p app/logs      # create nested folders
touch notes.txt        # create empty file
cp file1 file2         # copy file
mv file1 dir/          # move/rename
rm -rf temp/           # remove folder recursively
```

---

## 2) View File Content

**What:** Quickly read files.  
**Commands:**
```bash
cat file.txt                 # print whole file
less file.txt                # page-by-page view (q to quit)
head -n 20 file.txt          # first 20 lines
tail -n 100 file.txt         # last 100 lines
tail -f /var/log/syslog      # follow a log in real time
```

---

## 3) Find Text in Files (Your Interview Q)

**What:** Search a string inside files.  
**Commands:**
```bash
grep -R "ERROR" /var/log            # search recursively
grep -Rni "timeout" /etc/nginx      # -n line no, -i case-insensitive
grep -R --color "pattern" .         # highlight matches
```

**Example:** Find “Connection refused” in nginx configs  
```bash
grep -Rni "Connection refused" /etc/nginx
```

---

## 4) Find Files & Directories

**What:** Locate files by name/size/time.  
**Commands:**
```bash
find /var -name "*.log"                 # by name
find . -type f -size +100M              # files >100MB
find . -mtime -1                        # modified in last 1 day
```

---

## 5) Permissions & Ownership (Your Interview Q)

**What:** See permissions, change permissions, change owner.  
**Commands:**
```bash
ls -l file.txt                          # -rw-r--r--  user  group  file
chmod 640 file.txt                      # rw- r-- ---
chmod -R 750 /app                       # recursive change
chown user:group file.txt               # change owner
sudo chown -R appuser:appgrp /var/www   # recursive owner change
```

**Quick Permission Map:**  
- r=4, w=2, x=1 → sum to set numeric mode  
- `chmod 755` → owner rwx(7), group r-x(5), others r-x(5)

---

## 6) Processes & Services (Your Interview Q)

**What:** See running processes, resource usage, manage services.  
**Commands:**
```bash
ps aux | head                 # list all processes
ps aux | grep nginx           # filter by name
top                           # live CPU/mem view (q to quit)
htop                          # nicer top (if installed)
systemctl status nginx        # check a service
sudo systemctl restart nginx  # restart a service
journalctl -u nginx -n 100    # last 100 log lines for a service
journalctl -u nginx -f        # follow logs live
```

---

## 7) Logs (Your Interview Q – last 100 lines)

**Commands:**
```bash
tail -n 100 /var/log/messages
journalctl -u tomcat -n 100
journalctl -u tomcat -n 100 -f   # follow
```

---

## 8) Environment Variables (Your Interview Q)

**Temporary (current shell only):**
```bash
export APP_ENV=dev
echo $APP_ENV
```

**Permanent (for your user):**
```bash
echo 'export APP_ENV=dev' >> ~/.bashrc
source ~/.bashrc
```

**System-wide (all users):**
```bash
echo 'APP_ENV=dev' | sudo tee -a /etc/environment
```

---

## 9) Disk & Memory Usage (Your Interview Q)

**Disk space (mounted filesystems):**
```bash
df -h
```

**Directory usage + sort (descending):**
```bash
du -sh * 2>/dev/null | sort -h         # human sort by size (asc)
du -sm * 2>/dev/null | sort -nr        # MBs, numeric reverse (desc)
```

**Memory & load:**
```bash
free -h
uptime
```

---

## 10) Networking Basics

**Check ports & listeners:**
```bash
ss -tulpen                          # modern replacement for netstat
```

**Connectivity & HTTP checks:**
```bash
ping -c 4 google.com
curl -I http://localhost:8080
curl -vk https://example.com        # verbose TLS
```

**DNS lookup:**
```bash
dig example.com +short
nslookup example.com
```

---

## 11) Users & Groups

```bash
id                                   # current user info
whoami                               # user name
sudo useradd -m appuser
sudo passwd appuser
sudo usermod -aG docker appuser      # add to group
```

---

## 12) Compression & Archives

```bash
tar -czf logs.tgz /var/log/app       # create .tgz
tar -xzf logs.tgz                    # extract
unzip site.zip -d /var/www           # unzip to folder
```

---

## 13) Package Managers

```bash
# Debian/Ubuntu
sudo apt update && sudo apt install -y nginx

# RHEL/CentOS/Amazon Linux
sudo yum install -y nginx
```

---

## 14) File Editing & Quick Tricks

```bash
nano /etc/nginx/nginx.conf           # simple editor
vi /etc/nginx/nginx.conf             # vim

# Redirection & pipes
cat access.log | grep "200" | wc -l
echo "hello" | tee hello.txt
```

---

## 15) Cron (Schedule Jobs)

```bash
crontab -e                           # edit jobs
crontab -l                           # list jobs
# Example: run a script every night at 1:30 AM
30 1 * * * /usr/local/bin/backup.sh
```

---

## 16) System Info & OS Version

```bash
uname -a
cat /etc/os-release
hostnamectl
```

---

## 17) Symlinks vs Hardlinks (Common Interview)

```bash
ln -s /opt/app/config.yml /etc/app/config.yml   # symlink (pointer)
ln /var/log/app.log /tmp/app.log                # hardlink (same inode)
```

---

## 18) Real Troubleshooting Flow (You Can Say in Interviews)

**Scenario:** App not reachable on `http://server:8080`  
1. Check service & logs  
```bash
systemctl status myapp
journalctl -u myapp -n 100
```
2. Is it listening on the expected port?  
```bash
ss -tulpen | grep 8080
```
3. Is the firewall open?  
```bash
sudo iptables -L -n | less      # or: sudo ufw status
```
4. Any reverse proxy issues (nginx)?  
```bash
tail -n 100 /var/log/nginx/error.log
```
5. Check app logs directly  
```bash
tail -f /var/log/myapp/app.log
```

---

## 19) Mini Cheat Sheet (Copy-Paste)

```bash
# find string in files
grep -Rni "pattern" /path

# permissions & ownership
ls -l file ; chmod 640 file ; chown user:group file

# processes & services
ps aux | grep app ; systemctl status app ; journalctl -u app -n 100

# last 100 lines of a log
tail -n 100 /var/log/app.log

# env vars: temp & permanent
export ENV=dev ; echo 'export ENV=dev' >> ~/.bashrc ; source ~/.bashrc

# disk usage sorted desc
du -sm * 2>/dev/null | sort -nr | head

# network quick checks
ss -tulpen ; curl -I http://localhost:8080 ; ping -c 3 8.8.8.8
```

---

### ✅ Tips for a 2-Year DevOps Engineer
- Always start with **logs** (`journalctl`, `tail -f`) and **status** (`systemctl status`).  
- Use `-vvv` (very verbose) when tools support it (e.g., Ansible).  
- Keep aliases in `~/.bashrc` for frequent commands.  
- Practice `grep` + `find` + pipes — they solve 80% of day-to-day tasks.

---

**Prepared by:** Saravana L  
*(DevOps | Linux | Terraform | Ansible | Jenkins | Docker | GCP)*

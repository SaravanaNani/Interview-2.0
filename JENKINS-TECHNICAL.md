### What is a Jenkinsfile?
### What are the main sections inside a Jenkinsfile?
### Which type did you use: Declarative or Scripted? Why?

    ### Jenkinsfile
    A Jenkinsfile is a text file stored in SCM that defines the entire CI/CD pipeline as code.
### Main sections (Declarative Pipeline):
    pipeline
    agent
    stages
    stage
    steps
    environment
    post

### Scripted vs Declarative
Declarative: Structured, opinionated, easier to maintain
Scripted: Groovy-based, flexible, used for complex logic

### 2. Why do we use Maven?
    Maven is used for:
        Build automation
        Dependency management
        Standard project structure
        Integration with CI tools like Jenkins
### commands

    1️⃣ mvn clean
    
    - Deletes the target/ directory
    - Ensures a fresh build
    
    2️⃣ mvn compile : Compiles source code
    
    3️⃣ mvn test    : Runs unit tests using JUnit/TestNG
    
    4️⃣ mvn package : Compiles + tests + packages app (.jar / .war)
    
    5️⃣ mvn install : Installs artifact into local repository (~/.m2)
    
    6️⃣ mvn deploy  : Uploads artifact to remote repository (Nexus)
    
    💡 In your project:
    mvn clean package
    mvn deploy → Nexus : Jenkins pulls artifact from Nexus → deploys to Tomcat
# -
### Your Jenkins pipeline fails at SonarQube analysis stage, but the build works locally.
### How do you troubleshoot and fix this issue?

Step-by-step Troubleshooting
    
    1️⃣ Check Jenkins Console Logs
    
    Identify whether failure is:
    Authentication/token issue
    Quality Gate failure
    Scanner execution error
    
    2️⃣ Verify SonarQube Authentication
    
    Check if Sonar token is valid in Jenkins credentials
    Ensure correct SONAR_HOST_URL
    
    3️⃣ Check Sonar Scanner Configuration
    
    Validate sonar.projectKey
    Confirm correct pom.xml or sonar-project.properties
    
    4️⃣ Quality Gate Failure
    
    If Quality Gate fails:
    Build fails by design
    Review code smells, bugs, coverage
    Inform developers with report link
    
    5️⃣ Agent Issues
    
    Ensure Sonar Scanner is installed on Jenkins agent
    Verify Java version compatibility
    
    6️⃣ Re-run Locally
    
    Run mvn sonar:sonar locally using same config

  SonarQube Failure – Simplified Interview Answer

“If SonarQube analysis fails in Jenkins, I first check the console logs to identify whether it’s an authentication issue, scanner issue, or Quality Gate failure. 
I verify the Sonar token and server URL in Jenkins, validate the project key and configuration, 
and ensure the scanner and Java versions are compatible on the agent.
If the Quality Gate fails, the build is expected to fail, and I notify the developers with the Sonar report. 
I also re-run the scan locally to reproduce the issue.”

# - 

### What is a Jenkins agent?
    A Jenkins agent is a node or environment that executes Jenkins pipeline jobs. 
    It can be a physical server, VM, or container and allows Jenkins to distribute workloads.

### Why did you use Docker-based Jenkins agents?
### What problems do they solve?

    We used Docker-based Jenkins agents to provide isolated and consistent build environments across different stages. 
    They ensure the same tool versions are used every time, avoid dependency conflicts, scale easily, 
    and optimize cost by spinning up containers only when needed instead of maintaining dedicated EC2 instances.”
### what If a Jenkins pipeline fails without any code change, ?

    I check recent changes in the Jenkins environment such as plugin updates, agent availability, credential expiration,
    tool version changes (Java, Maven, Docker), or infrastructure issues. 
    I review the console logs to identify the failing stage, 
    verify connectivity to external systems like Git, Nexus, or SonarQube, and rerun the job on a clean workspace. 
    If needed, I rollback recent Jenkins or plugin changes.”

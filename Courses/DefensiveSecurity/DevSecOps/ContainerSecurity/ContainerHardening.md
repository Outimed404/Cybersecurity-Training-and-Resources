# Task 1 : Introduction 

## Overview
This module focuses on enhancing the security of Docker containers by addressing vulnerabilities and implementing best practices. 

### Learning Objectives
By the end of this module, you will be able to:
1. **Secure the Docker Daemon**: Restrict unauthorized access and interaction with the Docker daemon.
2. **Assign Proper Privileges**: Use capabilities to control container permissions effectively.
3. **Resource Management**: Prevent containers from exhausting system resources by applying limits.
4. **Use Security Features**:
   - **Seccomp**: Define system calls that a container is allowed to execute.
   - **AppArmor**: Set mandatory access controls to restrict container interactions with the operating system.
5. **Maintain Container Hygiene**:
   - Review Docker images for vulnerabilities.
   - Employ frameworks and tools to scan codebases and images for security risks.

---

## Prerequisites
Before starting, ensure familiarity with Docker basics, including its architecture and components. Completing the **Intro to Docker** room is highly recommended to grasp foundational concepts.

---

# Task 2 : Protecting the Docker Daemon


The Docker daemon plays a crucial role in managing containers, images, and associated operations. As it can be managed remotely, its security is paramount to prevent unauthorized access and potential exploits.

---

## The Importance of Securing the Docker Daemon

If attackers gain access to the Docker daemon, they can:
- Launch malicious containers.
- Interact with existing containers running sensitive applications, such as databases.
- Access and manipulate Docker images.

The Docker daemon is not exposed to the network by default but may be configured to do so, especially in **cloud environments** like CI/CD pipelines. Implementing secure communication and authentication methods is essential to mitigate risks.

---

## Methods to Secure Docker Daemon Communication

### 1. **SSH Authentication**

**Overview**:  
Using **SSH authentication**, developers can securely interact with remote devices running Docker. This approach uses **Docker contexts**, which are profiles allowing developers to manage configurations for different devices (e.g., development vs. production environments).

**Steps**:
1. **Create a Docker context**:
   ```bash
   docker context create \
   --docker host=ssh://myuser@remotehost \
   --description="Development Environment" \
   development-environment-host
   ```
2. **Using our newly created Docker context**:
    ```bash
    docker context use development-environment-host
    ```
To exit this context and, for example, use your own Docker engine, you can revert to "default" via `docker context use default.`

Note: This is not entirely secure. For example, a weak SSH password can lead to an attacker being able to authenticate. Strong password hygiene is strongly recommended. Some tips for a strong password have been included below:

- A high amount of characters (i.e. 12-22+)
- Special characters such as !, @, #, $
- Capital letters and numbers placed sporadically throughout (i.e. sUp3rseCreT!PaSSw0rd!)

Docker contexts allow you to interact with the Docker daemon directly over SSH, which is a secure and encrypted way of communication.

## TLS Encryption for Docker Daemon

**Overview**:  
Using **TLS encryption**, Docker can securely communicate over HTTP/S, enabling secure interaction with the Docker daemon from remote devices. This is particularly useful for integrating web services or applications that need to manage Docker containers on a remote host.

When configured in **TLS mode**, Docker will:
- Only accept remote commands from devices authenticated using valid certificates signed by a trusted Certificate Authority (CA).

---

## Key Considerations
1. **Certificate Management**:  
   Creating and managing TLS certificates involves setting expiry dates and encryption strength, which are environment-specific and beyond the scope of this guide.
   
2. **Security Limitations**:  
   - TLS does not inherently guarantee absolute security.  
   - Any device with a valid certificate and private key will be recognized as "trusted," so key and certificate protection is critical.

---

## TLS Configuration: Steps and Arguments

### 1. **On the Docker Host (Server)**  
Run the Docker daemon in TLS mode using the following command:
```bash
dockerd --tlsverify \
--tlscacert=myca.pem \
--tlscert=myserver-cert.pem \
--tlskey=myserver-key.pem \
-H=0.0.0.0:2376
```

### 3. **Telling DOcker to auth. using TLS**
```bash
docker --tlsverify \
--tlscacert=myca.pem \
--tlscert=client-cert.pem \
--tlskey=client-key.pem \
-H=SERVERIP:2376 info
```

**Note:** It is important to remember that this is not guaranteed to be secure. For example, anyone with a valid certificate and private key can be a "trusted" device. I have explained the arguments used in generating a TLS certificate and key in the table below:

| Argument       | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| `--tlscacert`  | This argument specifies the certificate of the certificate authority. A certificate authority is a trusted entity that issues the certificates used to identify devices. |
| `--tlscert`    | This argument specifies the certificate that is used to identify the device. |
| `--tlskey`     | This argument specifies the private key that is used to decrypt the communication sent to the device. |

### What would the command be if we wanted to create a Docker profile?
docker context create

### What would the command be if we wanted to switch to a Docker profile?
docker context use

# Task 3 : Implementing Control Groups

## Control Groups (cgroups)

Control Groups (also known as cgroups) are a feature of the Linux kernel that facilitates restricting and prioritizing the number of system resources a process can utilize.

For example, a process such as an application can be restricted to only use a certain amount of RAM or processing power, or given priority over other processes. This often improves system stability and allows administrators to track system resource use better.

In the context of Docker, implementing cgroups helps achieve isolation and stability. Because cgroups can be used to determine the number of (or prioritize) resources a container uses, this helps prevent faulty or malicious containers from exhausting a system. Of course, the best mechanism is preventing this from happening, but preventing a container from bringing down a whole system is an excellent second line of defense.

### Enabling cgroups in Docker

This behaviour is not enabled by default on Docker and must be enabled per container when starting the container. Below are the switches used to specify the limit of resources a container can use:

| Type of Resource | Argument                          | Example                                           |
|------------------|-----------------------------------|---------------------------------------------------|
| CPU              | `--cpus` (in core count)          | `docker run -it --cpus="1" mycontainer`           |
| Memory           | `--memory` (in k, m, g for kilobytes, megabytes, or gigabytes) | `docker run -it --memory="20m" mycontainer` |

You can also update the resource settings once the container is running using the `docker update` command. For example:
```bash

docker update --memory="40m" mycontainer
```

You can use the 
```bash
 docker inspect containername
``` 
 command to view information about a container (including the resource limits set). If a resource limit is set to 0, this means that no resource limits have been set.

 
 Docker uses namespaces to create isolated environments. For example, namespaces are a way of performing different actions without affecting other processes. Think of these as rooms in an office; each room serves its own individual purpose. What happens in a room in this office will not affect what happens in another office. These namespaces provide security by isolating processes from one another.

 ### What argument would we provide when running a Docker container to enforce how many CPU cores the container can utilise?
 --cpus

 ### What would the command be if we wanted to inspect a docker container named "Apache"?
 docker inspect apache

 # Task 4 : Preventing "Over - Privileged" Container


![DifferencesOfContainerModePrivileges](https://assets.tryhackme.com/additional/docker-rodeo/privileged-container/privileged-container-layers.png)

## Privileged Containers and Capabilities

### Privileged Containers

In the context of Docker, **privileged containers** are containers that have unchecked access to the host system. The entire point of containerization is to isolate the container from the host, but by running a container in "privileged" mode, the normal security mechanisms to isolate the container from the host are bypassed. 

While privileged containers can have legitimate use cases, such as running Docker-In-Docker (a container within a container) or for debugging purposes, they are extremely dangerous. 

When running a Docker container in "privileged" mode, Docker will assign **all possible capabilities** to the container, meaning the container can perform any action on the host system, including accessing sensitive resources like filesystems.

### What Are Capabilities?

**Capabilities** are a security feature of Linux that determines what processes can and cannot do at a granular level. Traditionally, processes either have full root privileges or no privileges at all, which can be risky. Capabilities provide a more fine-tuned approach, allowing you to control what privileges a process has. 

Below are some standard capabilities and their descriptions:

| **Capability**        | **Description**                                                                                                                                       | **Use Case**                                                                                             |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| **CAP_NET_BIND_SERVICE** | Allows services to bind to ports, specifically those under 1024, which typically require root privileges.                                               | Allowing a web server to bind on port 80 without root access.                                           |
| **CAP_SYS_ADMIN**         | Provides a variety of administrative privileges, such as mounting/unmounting filesystems, changing network settings, and performing system reboots.      | Modifying a user or starting/stopping a service in a process that automates administrative tasks.        |
| **CAP_SYS_RESOURCE**      | Allows a process to modify the maximum limit of resources available, such as increasing the memory or bandwidth a process can consume.                  | Controlling the amount of resources a process can consume, either increasing or reducing resources.      |

### Risks of Privileged Containers

To summarize, **privileged containers** are containers with full root access. This means they can bypass the isolation normally provided by Docker, potentially allowing attackers to escape the container and compromise the host system.

It's recommended to **assign capabilities individually** to containers rather than using the `--privileged` flag, which grants all possible capabilities. For example, if you're running a web server that needs to bind to port 80, you can use the following command to grant only the necessary capability:

```bash
docker run --cap-add=NET_BIND_SERVICE mycontainer

```
This allows the container to bind to ports below 1024 (such as port 80) without giving it full root access. By using specific capabilities, you can reduce the attack surface and minimize potential security risks.


```bash
docker run -it --rm --cap-drop=ALL --cap-add=NET_BIND_SERVICE mywebserver
```
``` bash
capsh --print
```

It is important to frequently review what capabilities are assigned to a container. When a container is privileged, it shares the same namespace as the host, meaning resources on the host can be accessed by the container - breaking the "isolated" environment.

### What is the name of the capability that allows services to bind to ports (specifically those under 1024)?
CAP_NET_BIND_SERVICE	

### What argument would we provide when starting a Docker container to add a capability?
--cap-add

### Finally, what command (with argument) would we use to print the capabilities assigned to a process?
capsh --print

# Task 5 :  Seccomp & AppArmor 101
## Seccomp

Seccomp is an important security feature of Linux that restricts the actions a program can and cannot do. To explain, picture a security guard at the entrance of an office. The security guard is responsible for making sure that only authorised people are allowed into the building and that they do what they are supposed to do. In this scenario, Seccomp is the security guard.

Seccomp allows you to create and enforce a list of rules for what actions (system calls) the application can make. For example, allowing the application to make a system call to read a file but not allowing it to make a system call to open a new network connection (such as a reverse shell).

These profiles are helpful because they reduce attackers' ability to execute malicious commands whilst maintaining the application's functionality.

### Example of a Seccomp Profile for a Web Server

Here is an example of a Seccomp profile that allows specific system calls for a web server:

```json
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "architectures": [
    "SCMP_ARCH_X86_64",
    "SCMP_ARCH_X86",
    "SCMP_ARCH_X32"
  ],
  "syscalls": [
    { "names": [ "read", "write", "exit", "exit_group", "open", "close", "stat", "fstat", "lstat", "poll", "getdents", "munmap", "mprotect", "brk", "arch_prctl", "set_tid_address", "set_robust_list" ], "action": "SCMP_ACT_ALLOW" },
    { "names": [ "execve", "execveat" ], "action": "SCMP_ACT_ERRNO" }
  ]
}
```

This Seccomp Porfile:

- Allows files to be read and written to.
- Allow a network socket to be created.
- Does not allow execution (e.g. execve).

## Creating a Custom Seccomp Profile
To create a Seccomp profile, you can simply create a profile using your favorite text editor. Here is an example of a Seccomp profile (profile.json) that allows reading and writing access to files but no network connections:

```json
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "architectures": ["SCMP_ARCH_X86_64"],
  "syscalls": [
    {
      "name": "socket",
      "action": "SCMP_ACT_ERRNO",
      "args": []
    },
    {
      "name": "connect",
      "action": "SCMP_ACT_ERRNO",
      "args": []
    },
    {
      "name": "bind",
      "action": "SCMP_ACT_ERRNO",
      "args": []
    },
    {
      "name": "listen",
      "action": "SCMP_ACT_ERRNO",
      "args": []
    },
    {
      "name": "accept",
      "action": "SCMP_ACT_ERRNO",
      "args": []
    },
    {
      "name": "read",
      "action": "SCMP_ACT_ALLOW",
      "args": []
    },
    {
      "name": "write",
      "action": "SCMP_ACT_ALLOW",
      "args": []
    }
  ]
}
```

## Applying the Seccomp Profile

With your Seccomp profile now created, you can apply it to your container at runtime by using the --security-opt seccomp flag with the location of the Seccomp profile. For example:
```bash
docker run --rm -it --security-opt seccomp=/home/cmnatic/container1/seccomp/profile.json mycontainer

```

Docker already applies a default Seccomp profile at runtime, but customizing it can help you harden the container further while maintaining functionality.

## AppArmor
AppArmor is a similar security feature in Linux because it prevents applications from performing unauthorised actions. However, it works differently from Seccomp as it is not included in the application but in the operating system.

AppArmor is a Mandatory Access Control (MAC) system that determines the actions a process can execute based on a set of rules at the operating system level.

### Checking if AppArmor is Installed
```bash
sudo aa-status
```
## Creating an AppArmor Profile
Below is an example of an AppArmor profile (profile.json) for an Apache web server:

```bash
/usr/sbin/httpd {
  capability setgid,
  capability setuid,

  /var/www/** r,
  /var/log/apache2/** rw,
  /etc/apache2/mime.types r,

  /run/apache2/apache2.pid rw,
  /run/apache2/*.sock rw,

  # Network access
  network tcp,

  # System logging
  /dev/log w,

  # Allow CGI execution
  /usr/bin/perl ix,

  # Deny access to everything else
  /** ix,
  deny /bin/**,
  deny /lib/**,
  deny /usr/**,
  deny /sbin/**
}
```
## Importing the AppArmor Profile
Once you have created your AppArmor profile, you will need to import it into AppArmor for it to be recognised:
```bash
sudo apparmor_parser -r -W /home/cmnatic/container1/apparmor/profile.json
```

##  Applying the AppArmor Profile
To apply the AppArmor profile to a container at runtime, use the --security-opt apparmor flag with the location of the AppArmor profile:
```bash
docker run --rm -it --security-opt apparmor=/home/cmnatic/container1/apparmor/profile.json mycontainer
```
Just like Seccomp, Docker applies a default AppArmor profile at runtime. However, it may not be suitable for your specific use case, and you can customize it to harden your container further.
## What's the Difference?

- **AppArmor** determines what resources an application can access (e.g., CPU, RAM, network interface, filesystem) and what actions it can take on those resources.
- **Seccomp** operates within the program itself and restricts what system calls the process can make (e.g., what parts of the CPU and operating system functions).


It is not an "either-or" scenario. Seccomp and AppArmor can be used together to create multiple layers of security for your containers.


### If we wanted to enforce the container to only be able to read files located in /home/tryhackme, what type of profile would we use? Seccomp or AppArmor?
AppArmor

### If we wanted to disallow the container from a system call (such as clock_adjtime), what type of profile would we use? Seccomp or AppArmor?
Seccomp

### Finally, what command would we use if we wanted to list the status of AppArmor?
aa-status

# Task 6 : Reviewing Docker Images

Reviewing Docker images is crucial for security, especially when considering the risks of running unknown code in a production environment. Just like you would be cautious about running unverified code on your device, the same caution should apply to Docker images.

## The Importance of Reviewing Docker Images

Malicious Docker images have caused significant damage in the past. For example, in 2020, Palo Alto discovered cryptomining Docker images that were pulled and run over two million times. This highlights the importance of auditing images before running them, especially in production environments.

## Docker Hub and Dockerfiles

Images on Docker Hub often come with the Dockerfiles that were used to build them. Docker Hub displays the layers of an image, which represent the steps executed during the build process. These layers give you insights into what commands were run to create the image, and help identify potential security concerns.

Open-source repositories on Docker Hub often include the full Dockerfile, allowing you to inspect the code and verify exactly what actions are being executed. By analyzing the Dockerfile, you can detect any vulnerabilities or malicious behaviors that may exist within the image.

## Tools for Auditing Docker Images

There are tools like Dive that allow you to reverse engineer Docker images by inspecting each layer of the image. Dive helps you understand what changes occurred at each step of the build process, making it easier to spot any malicious or unsafe operations.

In conclusion, auditing Docker images is a vital step in ensuring

# Task 7 : Compliance & Benchmarking
## Compliance and Benchmarking in Container Security

Compliance and benchmarking are critical for securing assets, particularly in containerized environments. These practices ensure that containers are built and run in accordance with established security standards and best practices.

## Compliance in Container Security

Compliance refers to adhering to regulatory standards and frameworks that outline security requirements. These frameworks provide guidelines and best practices to ensure that security measures are followed to protect sensitive data and systems.

### Key Compliance Frameworks

| **Compliance Framework** | **Description** | **URL** |
| ------------------------ | --------------- | ------- |
| **NIST SP 800-190** | A framework by the National Institute of Standards and Technology that provides guidance on container security concerns and solutions. | [NIST SP 800-190](https://csrc.nist.gov/publications/detail/sp/800-190/final) |
| **ISO 27001** | An international standard for information security, guiding the implementation, maintenance, and improvement of an information security management system. | [ISO 27001](https://www.iso.org/standard/27001) |

In addition to these, you may need to comply with other industry-specific frameworks such as:
- **HIPAA** (Health Insurance Portability and Accountability Act) for the medical industry.
- Regulations in other sectors, such as finance, that require adhering to their respective compliance standards.

## Benchmarking in Container Security

Benchmarking is the process of measuring how well an organization follows best practices and security standards. By benchmarking, organizations can identify areas where they are excelling and where improvements are necessary.

### Popular Benchmarking Tools

| **Benchmarking Tool** | **Description** | **URL** |
| --------------------- | --------------- | ------- |
| **CIS Docker Benchmark** | A tool to assess container compliance with the CIS Docker Benchmark framework. | [CIS Docker Benchmark](https://www.cisecurity.org/benchmark/docker) |
| **OpenSCAP** | A tool that can assess compliance with multiple frameworks, including CIS Docker Benchmark and NIST SP 800-190. | [OpenSCAP](https://www.open-scap.org/) |
| **Docker Scout** | A cloud-based service provided by Docker that scans Docker images and libraries for vulnerabilities. | [Docker Scout](https://docs.docker.com/scout/) |
| **Anchore** | A tool for assessing container compliance with multiple frameworks, including CIS Docker Benchmark and NIST SP 800-190. | [Anchore](https://github.com/anchore/anchore-engine) |
| **Grype** | A fast and modern vulnerability scanner for Docker images. | [Grype](https://github.com/anchore/grype) |

### Example of Docker Scout Usage

Docker Scout is a useful tool for analyzing Docker images and identifying vulnerabilities. To use Docker Scout, it must be installed beforehand. You can refer to the [Docker Scout documentation](https://docs.docker.com/scout/) for installation and usage instructions.

## Conclusion

By adhering to compliance frameworks and regularly benchmarking container security practices, organizations can ensure they are following industry standards and addressing potential vulnerabilities. Utilizing tools such as Docker Scout and OpenSCAP can help automate this process and maintain secure container environments.



```bash 
docker scout cves local://nginx:latest
    ✓ SBOM of image already cached, 215 packages indexed
    ✗ Detected 22 vulnerable packages with a total of 45 vulnerabilities

## Overview
                    │       Analyzed Image         
────────────────────┼──────────────────────────────
  Target            │  local://nginx:latest        
    digest          │  4df6f9ac5341                
    platform        │ linux/amd64                  
    vulnerabilities │    0C     1H    18M    28L   
    size            │ 91 MB                        
    packages        │ 215                          

## Packages and Vulnerabilities
   0C     1H     1M     3L  glibc 2.35-0ubuntu3.1
pkg:deb/ubuntu/glibc@2.35-0ubuntu3.1?os_distro=jammy&os_name=ubuntu&os_version=22.04
    ✗ HIGH CVE-2023-4911
      https://scout.docker.com/v/CVE-2023-4911
      Affected range : <2.35-0ubuntu3.4                              
      Fixed version  : 2.35-0ubuntu3.4                               
      CVSS Score     : 7.8                                           
      CVSS Vector    : CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H  
    
    ✗ MEDIUM CVE-2023-5156
      https://scout.docker.com/v/CVE-2023-5156
      Affected range : <2.35-0ubuntu3.5                              
      Fixed version  : 2.35-0ubuntu3.5                               
      CVSS Score     : 7.5                                           
      CVSS Vector    : CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H  
    
    ✗ LOW CVE-2016-20013
      https://scout.docker.com/v/CVE-2016-20013
      Affected range : >=0                                           
      Fixed version  : not fixed                                     
      CVSS Score     : 7.5                                           
      CVSS Vector    : CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H  
    
    ✗ LOW CVE-2023-4813
      https://scout.docker.com/v/CVE-2023-4813
      Affected range : <2.35-0ubuntu3.5                              
      Fixed version  : 2.35-0ubuntu3.5                               
      CVSS Score     : 5.9                                           
      CVSS Vector    : CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:N/I:N/A:H  
    
    ✗ LOW CVE-2023-4806
      https://scout.docker.com/v/CVE-2023-4806
      Affected range : <2.35-0ubuntu3.5                              
      Fixed version  : 2.35-0ubuntu3.5                               
      CVSS Score     : 5.9                                           
      CVSS Vector    : CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:N/I:N/A:H

    
```

### What is the name of the framework published by the National Institute of Standards and Technology?
NIST SP 800-190

### What is the name of the analysis tool provided by Docker? 
Docker Scout

# Task 8 : Pratical

### Use Docker to list the running containers on the system. What is the name of the container that is currently running?
couchdb

###  Use Grype to analyse the "struts2" image. What is the name of the library marked as "Critical"? 
struts2-core

### Use Grype to analyse the exported container filesystem located at /root/container.tar. What severity is the "CVE-2023-45853" rated as? 
Critical
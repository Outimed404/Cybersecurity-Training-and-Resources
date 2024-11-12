# Task 1 : Introduction

This room is the first of a series explaining the popular technology of containerisation. 

## Learning Outcomes:

By completing this room, you will know:

- What containerisation is and what containers are
- Where and why containerisation is used?
- A fundamental understanding of the popular containerisation technology called Docker
- What makes Docker so popular
- How containerisation works

# Task 2 : What is Containerisation

## Summary of Containerisation in Computing

**Containerisation** is the process of packaging an application along with its necessary resources (libraries, packages) into a single unit known as a **container**. This approach enhances the portability and ease of running applications by addressing key challenges associated with application dependencies.

### Key Points:
- Modern applications often rely on specific frameworks and libraries which:
  - Can be difficult to install across different environments or may not be supported by all operating systems.
  - Create challenges in diagnosing issues due to potential environment-related problems.
  - Can conflict with other software (e.g., different Python versions required by various applications).

**Containerisation platforms** (e.g., Docker) mitigate these issues by packaging dependencies and isolating the application's environment. This isolation ensures that if the containerisation engine is supported, the application behaves consistently.

### Isolation and Security:
- Containers use the **namespace** feature of the kernel, allowing processes to access OS resources without interacting with other processes.
- This isolation improves security since a compromised container generally does not affect others unless they share the same namespace.

### Comparison with Virtual Machines:
- **Virtual machines** require a full operating system to run applications, consuming more disk space, CPU, and RAM, unlike the lightweight approach of containers.

**Conclusion**: Containerisation simplifies application deployment, ensuring consistent environments while optimizing resource usage and security.


### What is the name of the kernel feature that allows for processes to use resources of the Operating System without being able to interact with other processes? 
namespace

### In a normal configuration, can other containers interact with each other? (yay/nay)
nay

# Task 3 : Introducing Docker

## Summary of Docker as a Containerisation Platform

**Docker** is an open-source, comprehensive containerisation platform that simplifies the deployment, management, and sharing of applications. It supports **Linux, Windows, and MacOS**, making it an adaptable choice for running applications efficiently.

### Key Features of Docker:
- **Application Images**: Docker allows applications to be packaged as “images” that can be easily shared and deployed. Users can download these images and run them seamlessly.
- **Docker Engine**: The core component that facilitates communication between the host OS and containers, enabling access to system hardware (CPU, RAM, networking, disk).
- **Connectivity**: Docker supports connecting multiple containers, such as pairing a web application container with a database container.
- **File Transfer**: Allows file transfers between the host operating system and containers.
- **Import/Export**: Simplifies exporting and importing application images for easy sharing and deployment.

### Docker Syntax and Orchestration:
- **YAML Syntax**: Docker uses YAML for defining how containers should be built and what processes to run, ensuring consistency and ease of debugging across different environments.
- **Container Orchestration**: Docker supports orchestrating multiple containers as part of a group, enabling communication between containers (e.g., a web server and a database container).

Docker’s capabilities make it a robust, portable, and easy-to-use platform for developing and running applications across various environments.

### What does an application become when it is published using Docker? Format: An xxxxx (fill in the x's) 
An Image

### What is the abbreviation of the programming syntax language that Docker uses?
YAML

# Task 4 : The History of Docker

## Brief History and Popularity of Docker

**Docker**, an open-source containerisation platform, was originally created by **Solomon Hykes** in 2013. It began as an internal project for **dotCloud**, a Platform-as-a-Service (PaaS) provider, and was first showcased at **PyCon 2013** before becoming open-source.

### Origins of Containerisation:
- **1979**: The concept of containerisation originated with **Unix V7**.
- **2013**: Docker brought containerisation into mainstream use, making its advantages widely accessible and modern.

### Docker's Popularity (as of April 2022):
- **13 million developers** actively use Docker [1].
- **7 million applications** are available and ready for use with Docker [2].
- **13 billion application downloads** occur monthly [3].
- These figures are based on data from the **official Docker repository**.

**References**:
- [1, 2] Dockerhub.com, April 2022
- [3] Docker.com, April 2022

### In what year was Docker originally created?
2013

### Where was Docker first showcased?
PyCon

### What version of Unix had the first concepts of containerisation?
v7

# Task 5 : The Benefits & Features of Docker

## Why Docker is a Great Choice for Application Deployment

Docker is an agile, convenient, and extensive platform for deploying applications. Here are some reasons why Docker stands out:

### Docker is Free
Docker is open-source and free to use. While business plans exist, you can download, use, create, run, and share Docker images without any cost.

### Docker is Compatible
Docker supports **Linux, macOS, and Windows**. Since containerisation isolates applications, if a device supports the Docker Engine, any container can run on it, regardless of the application or dependencies.

### Docker is Efficient & Minimal
Compared to alternatives like **virtual machines**, Docker is more efficient. Containers share minimal operating system images, which reduces storage requirements:
- **Example**: A minimal **Ubuntu container image** is only about **100MB**, while a full **Ubuntu server image** for VMs is about **1GB**.
  
Docker runs efficiently because the Docker Engine interacts directly with the host OS, eliminating the need for a separate operating system for each container.

### Docker is Easy to Get Started With
- Docker's developer documentation is comprehensive, with articles, examples, and FAQs available.
- The syntax is straightforward, and with many pre-published Docker images for various applications, getting started is fast.

### Docker is Easy to Share With Others
Docker images contain instructions on how containers should be built. These images can be easily exported, shared, and uploaded to **public or private repositories** like **DockerHub** or **GitHub**. As long as the Docker Engine supports the image, it can be run anywhere.

### Docker is Minimal
Docker images are designed to be minimal, which:
- Allows containers to be tailored exactly to the developer’s needs.
- Enhances security by reducing unnecessary packages, minimizing vulnerabilities.

### Docker is Cheaper to Run
Running Docker containers is usually cheaper than running virtual machines, especially in cloud environments:
- Containers require less memory and disk space since they don't run full operating systems.
- Virtual machines need specialized hardware for virtualisation and consume more resources.

For example, a **$5 VPS** can run several Docker containers but may not support virtual machines due to resource constraints.

**Conclusion**: Docker’s efficiency, compatibility, minimalism, ease of use, and cost-effectiveness make it a powerful solution for deploying and managing applications.


# Task 6 : How does Containerisation Work ?

## Understanding Namespaces and Containerisation

**Namespaces** in Linux are used to segregate system resources (such as processes, files, and memory) to ensure that they are isolated from each other.

### Key Concepts:
- Every process in Linux is assigned:
  - A **namespace**
  - A **process identifier (PID)**

Namespaces are essential to **containerisation**. Each container runs in its own namespace, meaning processes inside a container can only "see" other processes within the same namespace, preventing conflicts.

### Example: Docker Container vs Host Operating System
Consider a Docker container running a web server and compare it to the processes running on the host operating system. In the host OS:
- **Process #0**: The first process started when the system boots.
- **Process #1**: The **init process** (e.g., `systemd` in newer Ubuntu versions), which controls the rest of the system's processes.

### Potential Vulnerability: Escaping the Namespace
- **Containers use namespaces** to isolate processes. However, if a container's namespace coincides with the host OS’s processes, there is an opportunity to **escape the container** and potentially escalate privileges.

### Conclusion:
Namespaces help achieve isolation in containerisation by segregating resources between containers and the host system. However, improper configuration or vulnerabilities may lead to **namespace escape**, compromising the security of the system.

### What command can we use to view a list of running processes?
ps aux
# Task 7 : Pratical

ImageDocker

### Containerise the applications in the static site. What is the flag? 
THM{APPLICATION_SHIPPED}
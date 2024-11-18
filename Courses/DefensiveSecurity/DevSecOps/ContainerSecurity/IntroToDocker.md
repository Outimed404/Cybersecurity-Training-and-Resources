# Task 1 : Introduction

In this room, you’ll get your first hands-on experience deploying and interacting with Docker containers.

Namely, by the end of the room, you will be familiar with the following:

- The basic syntax to get you started with Docker
- Running and deploying your first container
- Understanding how Docker containers are distributed using images
- Creating your own image using a Dockerfile
- How Dockerfiles are used to build containers, using Docker Compose to orchestrate multiple containers
- Applying the knowledge gained from the room into the practical element at the end.

# Task 2 : Basic Docker Syntax

Docker can seem overwhelming at first. However, the commands are pretty intuitive, and with a bit of practice, you’ll be a Docker wizard in no time.

The syntax for Docker can be categorised into four main groups:

- Running a container
- Managing & Inspecting containers
- Managing Docker images
- Docker daemon stats and information

We will break down each of these categories in this task.

## Managing Docker Images

### Docker Pull
Before we can run a Docker container, we will first need an image. Recall from the “Intro to Containerisation” room that images are instructions for what a container should execute. There’s no use running a container that does nothing!

In this room, we will use the Nginx image to run a web server within a container. Before downloading the image, let’s break down the commands and syntax required to download an image. Images can be downloaded using the `docker pull` command and providing the name of the image.

For example, `docker pull nginx`. Docker must know where to get this image (such as from a repository which we’ll come onto in a later task).

Continuing with our example above, let’s download this Nginx image!

```bash
─$ sudo docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
2d429b9e73a6: Pull complete 
9b1039c85176: Pull complete 
9ad567d3b8a2: Pull complete 
773c63cd62e4: Pull complete 
1d2712910bdf: Pull complete 
4b0adc47c460: Pull complete 
171eebbdf235: Pull complete 
Digest: sha256:bc5eac5eafc581aeda3008b4b1f07ebba230de2f27d47767129a6a905c84f470
Status: Downloaded newer image for nginx:latest

```


| Docker Image | Tag      | Command Example              | Explanation                                                                                                                                                              |
|--------------|----------|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ubuntu       | latest   | docker pull ubuntu            | - IS THE SAME AS -                                                                                                                                                        |
|              |          | docker pull ubuntu:latest     | This command will pull the latest version of the "ubuntu" image. If no tag is specified, Docker will assume you want the "latest" version.                              |
|              |          |                              | It is worth remembering that you do not always want the "latest". This image is quite literally the "latest" in the sense it will have the most recent changes. This could either fix or break your container. |
| ubuntu       | 22.04    | docker pull ubuntu:22.04      | This command will pull version "22.04 (Jammy)" of the "ubuntu" image.                                                                                                   |
| ubuntu       | 20.04    | docker pull ubuntu:20.04      | This command will pull version "20.04 (Focal)" of the "ubuntu" image.                                                                                                   |
| ubuntu       | 18.04    | docker pull ubuntu:18.04      | This command will pull version "18.04 (Bionic)" of the "ubuntu" image.                                                                                                  |

#### Docker Image x/y/z

The docker image command, with the appropriate option, allows us to manage the images on our local system. To list the available options, we can simply do docker image to see what we can do. I’ve done this for you in the terminal below:

```bash
└─$ docker image            

Usage:  docker image COMMAND

Manage images

Commands:
  build       Build an image from a Dockerfile
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Display detailed information on one or more images
  load        Load an image from a tar archive or STDIN
  ls          List images
  prune       Remove unused images
  pull        Download an image from a registry
  push        Upload an image to a registry
  rm          Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Run 'docker image COMMAND --help' for more information on a command.


```

### Docker image ls

This command allows us to list all images stored on the local system. We can use this command to verify if an image has been downloaded correctly and to view a little bit more information about it (such as the tag, when the image was created and the size of the image).

```bash
└─$ sudo docker image ls
REPOSITORY   TAG          IMAGE ID       CREATED        SIZE
labs_web     latest       d2a5ebd6c1f4   5 weeks ago    488MB
nginx        latest       60c8a892f36f   6 weeks ago    192MB
mysql        8.0          f5da8fc4b539   3 months ago   573MB
php          7.4-apache   20a3732f422b   2 years ago    453MB
nginx        1.12.0       313ec0a602bc   7 years ago    107MB
```

### Docker image rm

Docker Image rm
If we want to remove an image from the system, we can use docker image rm along with the name (or Image ID). In the following example, I will remove the "ubuntu" image with the tag "8.0". My command will be docker image rm nginx:8.0 :

It is important to remember to include the tag with the image name.

```bash
└─$ sudo docker image rm nginx:1.12.0
Untagged: nginx:1.12.0
Untagged: nginx@sha256:aea0e686832d38c32a33ea6c6fcd0070598d7f09dce33d3bf7b2ce27b347f600
Deleted: sha256:313ec0a602bcdbff64e58bd5d83945b3d82ff8532e8275b4718c0a6c965a0795
Deleted: sha256:f852602e8d4c9c6c552aed5aca6c3245e9ef0c074c61fd5dfa922df42624d072
Deleted: sha256:65c8aae779f578067ae409941d1d3e30098dcbdb75ff002d6340c794deaac151
Deleted: sha256:54522c622682789028c72c5ba0b081d42a962b406cbc1eb35f3175c646ebf4dc
```

```bash
└─$ sudo docker image ls             
REPOSITORY   TAG          IMAGE ID       CREATED        SIZE
labs_web     latest       d2a5ebd6c1f4   5 weeks ago    488MB
nginx        latest       60c8a892f36f   6 weeks ago    192MB
mysql        8.0          f5da8fc4b539   3 months ago   573MB
php          7.4-apache   20a3732f422b   2 years ago    453MB
```

#### If we wanted to pull a docker image, what would our command look like?
docker pull

#### If we wanted to list all images on a device running Docker, what would our command look like?
docker image ls

#### Let's say we wanted to pull the image "tryhackme" (no quotations); what would our command look like?
docker pull tryhackme

#### Let's say we wanted to pull the image "tryhackme" with the tag "1337" (no quotations). What would our command look like?
docker pull tryhackme:1337

# Task 3 : Running Your First Container


The `docker run` command is used to create and start running containers from Docker images. It can include various options depending on the use case and the desired configuration of the container.

### Basic Syntax:
```bash
docker run [OPTIONS] IMAGE_NAME [COMMAND] [ARGUMENTS...]
```
- The `[OPTIONS]` part is optional and can be used to customize how the container runs.
- The `[COMMAND]` and `[ARGUMENTS]` are used to specify commands to run inside the container.

### Example: Running a Container Interactively
To run a container named `helloworld`, interactively with a bash shell:

To do this, I created a Dockerfile: 
```dockerfile
FROM alpine
RUN apk add --no-cache bash
CMD echo "Hello, World!"
CMD /bin/bash
```

I build it: 

```bash
└─$ sudo docker build -t helloworld .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  1.568GB
Step 1/4 : FROM alpine
 ---> 63b790fccc90
Step 2/4 : RUN apk add --no-cache bash
 ---> Running in 8e57eba2b803
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/community/x86_64/APKINDEX.tar.gz
(1/4) Installing ncurses-terminfo-base (6.4_p20240420-r2)
(2/4) Installing libncursesw (6.4_p20240420-r2)
(3/4) Installing readline (8.2.10-r0)
(4/4) Installing bash (5.2.26-r0)
Executing bash-5.2.26-r0.post-install
Executing busybox-1.36.1-r29.trigger
OK: 10 MiB in 18 packages
 ---> Removed intermediate container 8e57eba2b803
 ---> 6eb184ecbd8a
Step 3/4 : CMD echo "Hello, World!"
 ---> Running in 750e2c7ae41e
 ---> Removed intermediate container 750e2c7ae41e
 ---> b2d451994372
Step 4/4 : CMD /bin/bash
 ---> Running in 049f946f6b6b
 ---> Removed intermediate container 049f946f6b6b
 ---> 933f843e45bb
Successfully built 933f843e45bb
Successfully tagged helloworld:latest
```

Then I run using the interactive mode: 

```bash
└─$ sudo docker run -it helloworld   
64be2870c8e7:/# ls
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr
64be2870c8e7:/# exit
exit
```
I edited the Dockerfile and removed the last line in order to see if the interactive mode allow me to run the shell in my terminal: 

```dockerfile
FROM alpine
RUN apk add --no-cache bash
CMD echo "Hello, World!"
```
``` bash
┌──(kali㉿kali)-[~]
└─$ sudo docker build -t helloworld .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  1.568GB
Step 1/3 : FROM alpine
 ---> 63b790fccc90
Step 2/3 : RUN apk add --no-cache bash
 ---> Using cache
 ---> 6eb184ecbd8a
Step 3/3 : CMD echo "Hello, World!"
 ---> Using cache
 ---> b2d451994372
Successfully built b2d451994372
Successfully tagged helloworld:latest
                                                                                                                       
┌──(kali㉿kali)-[~]
└─$ sudo docker run -it helloworld /bin/bash
fcdb36f3a0fb:/# ls
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr
fcdb36f3a0fb:/# 

```

- The `-it` option combines `-i` (interactive) and `-t` (allocate a terminal).
- `/bin/bash` launches the bash shell inside the container.
- The container will start, and you can interact with it directly.

### Common Docker Run Options:

| OPTION   | Explanation                                                                 | Relevant Dockerfile Instruction | Example Command                                   |
|----------|-----------------------------------------------------------------------------|----------------------------------|---------------------------------------------------|
| `-d`     | Run the container in detached mode (background).                            | N/A                              | `docker run -d helloworld`                        |
| `-it`    | Run interactively and open a shell within the container.                    | N/A                              | `docker run -it helloworld`                       |
| `-v`     | Mount a host directory or file into the container.                         | `VOLUME`                         | `docker run -v /host/os/directory:/container/directory helloworld` |
| `-p`     | Bind a port on the host to a container's exposed port.                      | `EXPOSE`                         | `docker run -p 80:80 webserver`                   |
| `--rm`   | Automatically remove the container once it finishes running.               | N/A                              | `docker run --rm helloworld`                      |
| `--name` | Give the container a custom name instead of the default random name.        | N/A                              | `docker run --name helloworld`                    |

### Additional Options:
- You can set network adapters, specify container capabilities, or store values in environment variables.
- To explore more options, refer to the official Docker documentation.

### Listing Running Containers:
- To list containers that are currently running:

```bash
docker ps

└─$ sudo docker ps
[sudo] password for kali: 
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS     NAMES
23437d975ae1   nginx     "/docker-entrypoint.…"   10 seconds ago   Up 9 seconds   80/tcp    boring_kare
   

```

- To list all containers, including stopped ones:

```bash
docker ps -a

└─$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS                        PORTS                                                  NAMES
23437d975ae1   nginx          "/docker-entrypoint.…"    18 seconds ago   Up 18 seconds                 80/tcp                                                 boring_kare
86f5de1a1467   helloworld     "/bin/sh -c 'echo \"H…"   45 seconds ago   Exited (0) 44 seconds ago                                                            admiring_gates
e6a20ed4ef98   helloworld     "/bin/sh -c 'echo \"H…"   53 seconds ago   Exited (0) 52 seconds ago                                                            focused_clarke
fcdb36f3a0fb   helloworld    
```

### What would our command look like if we wanted to run a container interactively?
docker run -it

### What would our command look like if we wanted to run a container in "detached" mode?
docker run -d 

### Let's say we want to run a container that will run and bind a webserver on port 80. What would our command look like?
docker run -p 80:80

### How would we list all running containers?
docker ps

### How would we list all running containers? (including stopped)
docker ps -a

# Task 4 : Intro to Dockerfiles

Dockerfiles are essential for creating Docker images. They serve as an instruction manual, telling Docker what steps to take in building and configuring a container. Dockerfiles contain commands that execute actions when a container is built.

### Dockerfile Syntax
Dockerfiles are structured using the following syntax:

### Core Dockerfile Instructions:

| Instruction | Description                                                                                  | Example                               |
|-------------|----------------------------------------------------------------------------------------------|---------------------------------------|
| `FROM`      | Sets the base image for the container (operating system). All Dockerfiles must start with this. | `FROM ubuntu`                         |
| `RUN`       | Executes commands inside the container and creates a new layer.                              | `RUN whoami`                          |
| `COPY`      | Copies files from the local system to the container's working directory.                     | `COPY /home/cmnatic/myfolder/app/`    |
| `WORKDIR`   | Sets the working directory inside the container. Similar to the `cd` command in Linux.       | `WORKDIR /`                           |
| `CMD`       | Specifies the default command to run when the container starts. Usually used to start a service or app. | `CMD /bin/sh -c script.sh`           |
| `EXPOSE`    | Indicates which ports should be published when the container runs.                          | `EXPOSE 80`                           |

### Example Dockerfile:
The following Dockerfile will:

1. Use Ubuntu 22.04 as the base image.
2. Set the working directory to the root of the container.
3. Create a text file named `helloworld.txt`.

```Dockerfile
FROM ubuntu:22.04
WORKDIR /
RUN echo "Hello, World!" > helloworld.txt
```
This Dockerfile sets up a basic container with Ubuntu 22.04 and creates the helloworld.txt file in the root directory.

```bash
docker build -t helloworld .
```

```bash
docker run -it helloworld

└─$ sudo docker run -it helloworld2
root@539bc89e5ccf:/# ls
bin   dev  helloworld.txt  lib    lib64   media  opt   root  sbin  sys  usr
boot  etc  home            lib32  libx32  mnt    proc  run   srv   tmp  var
root@539bc89e5ccf:/# cat helloworld.txt 
Hello, World!
root@539bc89e5ccf:/# exit
exit

```

## Leveling Up Our Dockerfile

Let's enhance our Dockerfile to make the container more useful. Previously, our container only created a file. Now, we will:

1. Use **Ubuntu 22.04** as the base operating system.
2. Install the **Apache2** web server.
3. Configure networking by exposing port 80 for web access.
4. Set the container to start the **Apache2** service at startup.


```Dockerfile
# THIS IS A COMMENT
FROM ubuntu:22.04

# Update the APT repository to ensure we get the latest version of apache2
RUN apt-get update -y 

# Install apache2
RUN apt-get install apache2 -y

# Tell the container to expose port 80 to allow us to connect to the web server
EXPOSE 80 

# Tell the container to run the apache2 service
CMD ["apache2ctl", "-D","FOREGROUND"]
```
For reference, the command to build this would be docker build -t webserver . (assuming the Dockerfile is in the same directory as where you run the command from). Once starting the container with the appropriate options (docker run -d --name webserver -p 80:80  webserver), we can navigate to the IP address of our local machine in our browser!

## Optimising Our Dockerfile

There’s certainly an art to Docker - and it doesn’t stop with Dockerfiles! Firstly, we need to ask ourselves why is it essential to optimise our Dockerfile? Bloated Dockerfiles are hard to read and maintain and often use a lot of unnecessary storage! For example, you can reduce the size of a docker image (and reduce build time!) using a few ways:

1. Only installing the essential packages. What’s nice about containers is that they’re practically empty from the get-go - we have complete freedom to decide what we want.
2. Removing cached files (such as APT cache or documentation installed with tools). The code within a container will only be executed once (on build!), so we don’t need to store anything for later use.
3. Using minimal base operating systems in our FROM instruction. Even though operating systems for containers such as Ubuntu are already pretty slim, consider using an even more stripped-down version (i.e. ubuntu:22.04-minimal). Or, for example, using Alpine (which can be as small as 5.59MB!).
4. Minimising the number of layers - I’ll explain this further below.

Each instruction (I.E. FROM, RUN, etc.) is run in its own layer. Layers increase build time! The objective is to have as few layers as possible. For example, try chaining commands from RUN together like so:

Before:

```dockerfile
FROM ubuntu:latest
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install apache2 -y
RUN apt-get install net-tools -y
```

After:

```dockerfile
FROM ubuntu:latest
RUN apt-get update -y && apt-get upgrade -y && apt-get install apache2 -y && apt-get install net-tools
```

This is just a tiny example of a Dockerfile, so the build time will not be so drastic, but in much larger Dockerfiles - reducing the number of layers will have a fantastic performance increase during the build.

### What instruction would we use to specify what base image the container should be using?
FROM

### What instruction would we use to tell the container to run a command?
RUN

### What docker command would we use to build an image using a Dockerfile?
build

### Let's say we want to name this image; what argument would we use?
-t

# Task 5 : Docker Compose

Docker Compose is a tool that allows you to define and manage multi-container Docker applications. It enables containers to interact with each other while running in isolation. This is particularly useful when applications require multiple services, such as a web server and a database, to run together.

## Why Use Docker Compose?

- **Multi-container applications:** Many modern applications consist of multiple services (e.g., a web server and a database), which need to work together. Docker Compose helps orchestrate these services.
- **Efficient management:** Instead of managing containers individually, Docker Compose lets you define them in a single configuration file (`docker-compose.yml`), simplifying the process of starting, stopping, and managing multi-container setups.

## Docker Compose Fundamentals

### Prerequisites:
- Docker Compose must be installed (it doesn't come with Docker by default).
- You need a valid `docker-compose.yml` file to define your services.

### Common Docker Compose Commands:

| Command              | Explanation                                                                 | Example              |
|----------------------|-----------------------------------------------------------------------------|----------------------|
| `up`                 | Creates/starts containers as defined in the compose file.                   | `docker-compose up`  |
| `start`              | Starts containers that are already created.                                 | `docker-compose start`|
| `down`               | Stops and removes containers, networks, and volumes.                        | `docker-compose down`|
| `stop`               | Stops running containers without removing them.                             | `docker-compose stop`|
| `build`              | Builds images without starting the containers.                              | `docker-compose build`|

## Example Scenario: E-commerce Website with Apache and MySQL

In this scenario, you have an e-commerce website running on **Apache**, which stores customer information in a **MySQL database**.

### Without Docker Compose, you would:
1. Create a network between containers:
   ```bash
   docker network create ecommerce
    ```
2. Run the Apache web server container:
```bash
docker run -p 80:80 --name webserver --net ecommerce webserver
```
3. RUn the MySql database container:
```bash
docker run --name database --net ecommerce mysql
```
#### With Docker Compose:

Instead of manually managing each container, you can define both services (Apache and MySQL) and the network in a docker-compose.yml file, allowing you to manage everything with a single command.

```yaml
version: '3.8'
services:
  webserver:
    image: httpd:latest  # Apache web server
    networks:
      - ecommerce
    ports:
      - "80:80"
    
  database:
    image: mysql:latest
    networks:
      - ecommerce
    environment:
      - MYSQL_DATABASE=ecommerce
      - MYSQL_USERNAME=root
      - MYSQL_ROOT_PASSWORD=helloword
      - ecommerce

networks:
  ecommerce:
    driver: bridge
```

Start the services:
```bash
docker-compose up
```

Stop the services:
```bash
docker-compose down
```


## Key Instructions in a `docker-compose.yml` File

| **Instruction**  | **Explanation** | **Example** |
|------------------|-----------------|-------------|
| `version`        | Specifies the version of Docker Compose syntax used in the file. It should be the first line. | `'3.3'` |
| `services`       | Marks the beginning of container definitions in the file. | `services:` |
| `name`           | Defines the name of the container/service. | `webserver:` |
| `build`          | Points to the directory containing the Dockerfile to build the container. | `./webserver` |
| `ports`          | Exposes ports between the container and host system. | `'80:80'` |
| `volumes`        | Mounts directories from the host system to the container. | `'./webserver/:/var/www/html'` |
| `environment`    | Passes environment variables into the container (use for things like passwords). | `MYSQL_ROOT_PASSWORD=helloworld` |
| `image`          | Specifies the image to use for building the container. | `mysql:latest` |
| `networks`       | Defines the networks containers will connect to. Containers can connect to multiple networks. | `ecommerce` |

> **Note:** These are just some of the basic instructions. For a comprehensive list, refer to the official [Docker Compose documentation](https://docs.docker.com/compose/compose-file/).

---

### I want to use docker-compose  to start up a series of containers. What argument allows me to do this?
up

### I want to use docker-compose  to delete the series of containers. What argument allows me to do this?
down

### What is the name of the .yml file that docker-compose uses?
docker-compose.yml

# Task 6 : Intro to the docker socket
# Docker's Architecture

When you install Docker, two key programs are installed:

- **Docker Client**
- **Docker Server**

Docker operates in a **client-server** model, where the two programs communicate with each other to form the Docker platform we use today. This communication happens through **sockets**, which are a fundamental feature of the operating system that allow processes to communicate with each other.

### Sockets and Communication

Sockets are used for **Interprocess Communication (IPC)**, meaning that different processes running on an operating system can exchange data. 

For example, imagine a chat application that uses two sockets:

- A socket for sending messages.
- A socket for receiving messages.

Each socket stores the data (messages in this case) that the program needs to interact with. A socket can represent either a **network connection** or a **file**.

### Docker Communication via Sockets

In Docker's case, the communication works as follows:

1. **Docker Client**: Sends requests to the Docker Server.
2. **Docker Server**: Listens for requests from the Docker Client and processes them via an API.

For example, if you run the following command :

```bash
docker run helloworld
```

## Docker Client and Server Interaction

- The **Docker Client** sends a request to the **Docker Server** to run a container using the `"helloworld"` image.

---

## Docker Server API

- The **Docker Server** acts as an API that listens for requests and processes them accordingly. This API is at the core of Docker's functionality.

> **Note**: You can also interact with the Docker Server directly using tools like **`curl`** or API development tools like **Postman**.

---

## Demonstration of Docker Communication

An example of interacting with the Docker API is by using **Postman** to list all images stored on the system. This demonstrates how you can send requests to the Docker Server directly via the API.

---

## Docker Security Implications

Since Docker relies on API communication via sockets, the host machine running Docker can be configured to accept requests from other devices.

> **Security Warning**: If not configured properly, this could be a vulnerability because it allows remote devices to:
- Start/stop containers.
- Access container data.
- Execute commands within containers.

While this is a potential security risk, it can also be a useful feature in specific use cases (e.g., remote Docker management).

---

## Summary

- Docker operates using a **client-server** architecture.
- The **Docker Client** sends commands, and the **Docker Server** processes them.
- Communication occurs through **sockets** that enable **Interprocess Communication (IPC)**.
- Docker's API can be accessed using tools like **Postman** or **curl**.
- Improper configuration of Docker’s remote access can expose security vulnerabilities, but it also has useful applications.

# Task 7 : Pratical

### Connect to the machine. What is the name of the container that is currently running?
CloudIsland

### After starting the container, try to connect to https://10-10-95-196.p.thmlabs.com/ in your browser. What is the flag?
THM{WEBSERVER_CONTAINER}

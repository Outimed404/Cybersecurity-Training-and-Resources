# Task 1 : Introduction

In this room, you’ll get your first hands-on experience deploying and interacting with Docker containers.

Namely, by the end of the room, you will be familiar with the following:

- The basic syntax to get you started with Docker
- Running and deploying your first container
- Understanding how Docker containers are distributed using images
- Creating your own image using a Dockerfile
- How Dockerfiles are used to build containers, using Docker Compose to orchestrate multiple containers
- Applying the knowledge gained from the room into the practical element at the end.

# Task 2 : 

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

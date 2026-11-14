# Docker Fundamentals – Practical Lab 1

**Date:** 14 November 2026  
**Environment:** Windows 11, WSL2 Ubuntu, VS Code and Docker Desktop

## 1. Introduction to Docker

Docker is a platform used to package and run applications in containers.

Containers allow applications to run consistently across different environments.

### Important Docker Terminology

- **Docker Engine:** Manages Docker images and containers.
- **Docker Image:** A packaged template used to create containers.
- **Docker Container:** An instance created from an image.
- **Docker Hub:** An online registry for storing and sharing Docker images.
- **Docker Desktop:** A graphical application for managing Docker on a local computer.

## 2. Verify Docker Installation

Check Docker CLI:

```bash
docker --version
```

Check whether Docker Engine is running:

```bash
docker info
```

List all containers:

```bash
docker ps -a
```

List available images:

```bash
docker images
```

## 3. Working with Docker Images

Download the Nginx image:

```bash
docker pull nginx
```

Inspect an image:

```bash
docker image inspect nginx
```

Important information discovered:

- Image name: nginx
- Tag: latest
- Operating system: Linux
- Architecture: amd64
- Exposed port: 80/tcp
- Default command: nginx -g daemon off;

## 4. Creating and Running a Container

Create and start an Nginx container:

```bash
docker run -d --name dorothy-nginx -p 8080:80 nginx
```

Explanation:

- `docker run`: Creates and starts a container.
- `-d`: Runs the container in detached mode.
- `--name`: Assigns a container name.
- `-p 8080:80`: Maps laptop port 8080 to container port 80.
- `nginx`: Specifies the image.

## 5. Checking Container Status

Check running containers:

```bash
docker ps
```

Check all containers:

```bash
docker ps -a
```

## 6. Accessing the Nginx Web Server

Open the following address in a browser:

```text
http://localhost:8080
```

The Nginx welcome page confirmed that the web server was running successfully.

### Understanding Port Mapping

Port mapping connects a port on the host computer to a port inside the container.

In this lab:

- Host port: 8080
- Container port: 80
- Application: Nginx web server

## 7. Important Lessons Learned

1. One Docker image can create multiple containers.
2. Images remain available even after containers are removed.
3. Stopping a container is different from removing it.
4. Detached containers run independently of the browser.
5. `docker ps` displays running containers only.
6. `docker ps -a` displays running and stopped containers.
7. Docker port mapping makes applications inside containers accessible from the host.

## 8. Practical Project Status

- Container name: dorothy-nginx
- Image: nginx:latest
- Status: Running
- Browser access: http://localhost:8080
- Result: Successfully deployed an Nginx web server using Docker.

## 9. Next Learning Objectives

- Stop and restart containers.
- Understand the container lifecycle.
- Examine container logs.
- Execute commands inside running containers.
- Understand Docker volumes and persistent storage.
- Build custom Docker images using Dockerfiles.
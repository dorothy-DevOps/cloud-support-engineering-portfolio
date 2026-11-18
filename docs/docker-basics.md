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


## Nginx Configuration

Nginx uses its configuration file at:

`/etc/nginx/conf.d/default.conf`

Important directives:

- `listen 80;` - Nginx listens on port 80 inside the container.
- `server_name localhost;` - identifies the server name.
- `location /` - defines how requests to the website root are handled.
- `root /usr/share/nginx/html;` - specifies the website's document root.
- `index index.html index.htm;` - specifies the default index files.

### Docker Port Mapping

Our container was created using:

`docker run -d --name dorothy-nginx -p 8080:80 nginx`

The port mapping follows this format:

`-p HOST_PORT:CONTAINER_PORT`

Therefore:

- Host port: 8080
- Container port: 80

### Custom Webpage

I replaced the default Nginx `index.html` with my own HTML webpage inside:

`/usr/share/nginx/html/`

I verified that Nginx served my custom webpage at `http://localhost:8080`.

### Key Learning

Nginx configuration determines where website files are located. The actual files must exist in the configured document root for Nginx to serve them.


## Docker Bind Mounts

A bind mount connects a directory or file on the host machine to a location inside a Docker container.

### Bind Mount Practice

Created a website folder on the host machine:

`cloud-support-engineering-portfolio/website/`

Created an Nginx container with a bind mount:

```bash
docker run -d \
  --name dorothy-nginx-bindmount \
  -p 8081:80 \
  --mount type=bind,source=/mnt/c/Users/User/cloud-support-engineering-portfolio/website,target=/usr/share/nginx/html \
  nginx
```

### Understanding the Command

- `-d` runs the container in the background.
- `--name` assigns the container a name.
- `-p 8081:80` maps host port 8081 to container port 80.
- `type=bind` specifies a bind mount.
- `source` identifies the website folder on the host.
- `target` identifies the destination inside the container.

### Live Website Update

Edited `website/index.html` directly in VS Code.

After saving and refreshing the browser at `http://localhost:8081`, Nginx displayed the updated webpage without restarting the container or rebuilding the image.

### Key Learning

Bind mounts allow developers to modify application files on the host machine while the application runs inside a container.

This is particularly useful during local application development.


## Docker Volumes and Persistent Storage

A Docker volume is a storage mechanism managed by Docker. It allows data to persist independently of a container's lifecycle.

Unlike a bind mount, where the developer specifies a host directory, Docker manages the storage location of a named volume.

### 1. Creating a Docker Volume

Created a named volume:

```bash
docker volume create dorothy-data
```

List available volumes:

```bash
docker volume ls
```

Inspect a volume:

```bash
docker volume inspect dorothy-data
```

Docker reported the volume's mountpoint as:

`/var/lib/docker/volumes/dorothy-data/_data`

### 2. Attaching a Volume to a Container

Created an Ubuntu container with the volume attached:

```bash
docker run -it --name dorothy-volume-test \
  --mount source=dorothy-data,target=/data \
  ubuntu bash
```

- `source=dorothy-data` identifies the existing Docker volume.
- `target=/data` specifies where the volume is mounted inside the container.
- `/data` is the directory through which the container accesses the volume.

### 3. Creating Persistent Data

Inside the container, created a file:

```bash
echo "My first Docker volume stores persistent data" > /data/important.txt
```

Read the file:

```bash
cat /data/important.txt
```

Verified the file:

```bash
ls -l /data
```

### 4. Testing Volume Persistence

Exited the first container:

```bash
exit
```

Removed the stopped container:

```bash
docker rm dorothy-volume-test
```

Verified that the volume still existed:

```bash
docker volume ls
```

Created a new container using the same volume:

```bash
docker run -it --name dorothy-volume-test-2 \
  --mount source=dorothy-data,target=/data \
  ubuntu bash
```

Retrieved the original file:

```bash
cat /data/important.txt
```

The original content was successfully retrieved.

**Result:** Data stored in the named volume survived the removal of the original container.

## Docker Volume Backup and Recovery

Persistent storage does not replace backups. If a volume is deleted or its underlying storage is damaged, data may be lost.

### 1. Creating a Backup Directory

Created a backup directory in the project:

```bash
mkdir -p docker-backups
```

### 2. Backing Up a Docker Volume

Used a temporary Ubuntu container to create a compressed archive:

```bash
docker run --rm \
  --mount source=dorothy-data,target=/data,readonly \
  --mount type=bind,source="$(pwd)/docker-backups",target=/backup \
  ubuntu \
  tar -czf /backup/dorothy-data-backup.tar.gz -C /data .
```

Explanation:

- `--rm` automatically removes the temporary container after execution.
- `readonly` protects the original volume from modifications by the backup container.
- The bind mount makes the host backup directory accessible inside the container.
- `tar -czf` creates a compressed archive.
- `-C /data .` archives the contents of the volume.

### 3. Verifying the Backup

Listed the backup file:

```bash
ls -lh docker-backups
```

Listed the archive contents:

```bash
tar -tzf docker-backups/dorothy-data-backup.tar.gz
```

The archive contained:

- `important.txt`

### 4. Restoring the Backup

Created a separate recovery volume:

```bash
docker volume create dorothy-recovery
```

Restored the backup into the new volume:

```bash
docker run --rm \
  --mount type=bind,source="$(pwd)/docker-backups",target=/backup,readonly \
  --mount source=dorothy-recovery,target=/restore \
  ubuntu \
  tar -xzf /backup/dorothy-data-backup.tar.gz -C /restore
```

### 5. Verifying Recovery

Checked the restored file:

```bash
docker run --rm \
  --mount source=dorothy-recovery,target=/restore,readonly \
  ubuntu \
  cat /restore/important.txt
```

Successfully recovered the original content:

`My first Docker volume stores persistent data`

**Result:** Successfully backed up a Docker volume and restored its data into a separate volume.

## Key Lessons Learned

1. Containers and volumes have separate lifecycles.
2. Named volumes preserve data when their containers are removed.
3. A new container can reuse an existing named volume.
4. Bind mounts connect host directories to containers.
5. Docker volumes are managed by Docker.
6. Persistent storage is not the same as a backup.
7. Backups should be stored separately from the original data.
8. Recovery should be tested, not merely assumed.

## Practical Lab Resources

- Original volume: `dorothy-data`
- Recovery volume: `dorothy-recovery`
- Test file: `important.txt`
- Backup directory: `docker-backups/`
- Backup archive: `dorothy-data-backup.tar.gz`

## Next Learning Objectives

- Build custom Docker images using Dockerfiles.
- Understand image layers and build caching.
- Tag and manage custom images.
- Publish images to Docker Hub.

## 7. Building Custom Docker Images with Dockerfiles

A Dockerfile is a text file containing instructions Docker uses to build a custom image.

### Dockerfile used

```dockerfile
FROM nginx:alpine
WORKDIR /usr/share/nginx/html
COPY website/ .
EXPOSE 80
```

### Build the custom image

```bash
docker build -t dorothy-portfolio-website:v1 .
```

### Run a container from the custom image

```bash
docker run -d --name dorothy-custom-website -p 8082:80 dorothy-portfolio-website:v1
```

The website files were copied into the image during the build process.

## 8. Docker COPY vs Bind Mounts

### COPY

`COPY` copies files from the host into the image during image building.

Changes made to the original host files do not automatically appear in existing containers.

To apply changes, rebuild the image and create a new container.

### Bind Mount

A bind mount connects a directory on the host machine to a directory inside a container.

Example:

```bash
docker run -d \
  --name dorothy-dev-website \
  -p 8084:80 \
  -v "$(pwd)/website:/usr/share/nginx/html" \
  dorothy-portfolio-website:v1
```

This allowed me to edit `website/index.html` directly in VS Code and see the changes in my browser without rebuilding the image or restarting the container.

### Key Differences

| COPY | Bind Mount |
|---|---|
| Copies files into the image | Connects host files to a container directory |
| Used during image building | Used when creating or running a container |
| Changes require a new image and container | Changes are reflected immediately when saved |
| Commonly used for production images | Very useful for local development |

**Practical lesson:** Bind mounts are useful during development, while COPY is commonly used to package application files into production images.

## Docker Image Layers and Build Cache

### Docker Image Layers
Docker builds images using layers. Each Dockerfile instruction can create a layer or contribute metadata to the image.

For my portfolio website, the Dockerfile contains:

```dockerfile
FROM nginx:alpine
WORKDIR /usr/share/nginx/html
COPY website/ .
EXPOSE 80
```

- `FROM` provides the base image.
- `WORKDIR` sets the working directory.
- `COPY` copies website files into the image.
- `EXPOSE` documents the container's expected listening port.

### Docker Build Cache
Docker uses build cache to reuse previous build results and make image builds faster.

I practised building an image using:

```bash
docker build -t dorothy-portfolio-website:cache-test .
```

I observed Docker reuse cached build steps.

### Disabling Build Cache

I also tested:

```bash
docker build --no-cache -t dorothy-portfolio-website:no-cache-test .
```

The `--no-cache` option prevents reuse of cached build layers.

### Key Lessons
1. Docker images consist of layers.
2. Build cache improves build speed and efficiency.
3. Changes to files used by `COPY` can invalidate its cache.
4. `--no-cache` is useful when troubleshooting or when a fresh execution of build steps is required.
5. Docker build output helps identify cached and newly executed steps.

**Practical project:** `dorothy-portfolio-website`

## Docker Build Context and .dockerignore Experiment

### Objective
Understand how `.dockerignore` controls which files Docker can access during an image build.

### Experiment
I created a temporary Dockerfile that explicitly requested both the website and screenshots directories:

```dockerfile
FROM nginx:alpine
WORKDIR /usr/share/nginx/html
COPY website/ .
COPY screenshots/ /screenshots/
EXPOSE 80
```

### Test 1: Screenshots excluded

With `screenshots/` listed in `.dockerignore`, Docker failed at:

```dockerfile
COPY screenshots/ /screenshots/
```

Docker reported a `CopyIgnoredFile` warning because the screenshots directory was excluded from the build context.

### Test 2: Screenshots included

After temporarily removing `screenshots/` from `.dockerignore`, the build succeeded.

In the latest test, BuildKit reported a build-context transfer of approximately 218.39 KB.

### Findings

1. `.dockerignore` excludes specified files and directories from the Docker build context.
2. Docker cannot copy files that have been excluded from the build context.
3. BuildKit can optimise context transfer by sending only files needed by the build.
4. Unnecessary files should not be included in production images.
5. `.dockerignore` and `.gitignore` serve different purposes: one controls Docker build context, while the other controls Git tracking.

### Conclusion

I demonstrated how `.dockerignore` prevents unwanted files from entering a Docker build context. I also observed how BuildKit optimises context transfer based on the files requested by the Dockerfile.
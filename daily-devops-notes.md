
# My Daily DevOps Command Journal

**Learning path:** Cloud Architecture and DevOps  
**Environment:** Windows 11, WSL2 Ubuntu, VS Code, Docker Desktop

---

## 1. Docker Installation and Engine

| Command | Purpose |
|---|---|
| `docker --version` | Check Docker CLI version |
| `docker info` | Check Docker Engine information |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers, including stopped ones |
| `docker images` | List downloaded images |

## 2. Docker Images

| Command | Purpose |
|---|---|
| `docker pull nginx` | Download the Nginx image |
| `docker pull ubuntu` | Download the Ubuntu image |
| `docker images` | View available images |
| `docker image inspect nginx` | Inspect image configuration |
| `docker image ls ubuntu` | List Ubuntu images |

## 3. Container Management

| Command | Purpose |
|---|---|
| `docker run -it ubuntu bash` | Create and enter an interactive Ubuntu container |
| `docker run -d nginx` | Run Nginx in the background |
| `docker start CONTAINER_NAME` | Start an existing stopped container |
| `docker stop CONTAINER_NAME` | Stop a running container |
| `docker rm CONTAINER_NAME` | Remove a stopped container |
| `docker logs CONTAINER_NAME` | View container logs |
| `docker exec -it CONTAINER_NAME sh` | Enter a running container |
| `docker inspect CONTAINER_NAME` | View detailed container information |

## 4. Port Mapping

Example:

`docker run -d --name dorothy-nginx -p 8080:80 nginx`

- `8080` = port on my laptop (host).
- `80` = port inside the container.
- `-d` = run in detached/background mode.
- `--name` = give the container a name.

Browser address: `http://localhost:8080`

## 5. Bind Mounts

Example:

`--mount type=bind,source=/host/folder,target=/container/folder`

- Connects a folder on my laptop to a folder inside a container.
- Changes made to the host folder can appear immediately inside the container.
- Useful for local development.

## 6. Docker Volumes

| Command | Purpose |
|---|---|
| `docker volume ls` | List Docker volumes |
| `docker volume create dorothy-data` | Create a named volume |
| `docker volume inspect dorothy-data` | Inspect a volume |
| `docker volume rm VOLUME_NAME` | Remove a volume (only when safe and no longer needed) |

Attach a volume:

`docker run -it --name dorothy-volume-test --mount source=dorothy-data,target=/data ubuntu bash`

- `dorothy-data` = Docker-managed volume.
- `/data` = mount point inside the container.
- Files stored in `/data` persist independently of the container.

## 7. Docker Volume Backup

Backup command:

```bash
docker run --rm \
  --mount source=dorothy-data,target=/data,readonly \
  --mount type=bind,source="$(pwd)/docker-backups",target=/backup \
  ubuntu \
  tar -czf /backup/dorothy-data-backup.tar.gz -C /data .
```

- `--rm` removes the temporary backup container after it exits.
- `readonly` protects the source volume from changes during backup.
- `tar -czf` creates a compressed archive.
- `-C /data .` archives the contents of the mounted volume.

Verify backup:

`tar -tzf docker-backups/dorothy-data-backup.tar.gz`

## 8. Docker Volume Recovery

Create a recovery volume:

`docker volume create dorothy-recovery`

Restore command:

```bash
docker run --rm \
  --mount type=bind,source="$(pwd)/docker-backups",target=/backup,readonly \
  --mount source=dorothy-recovery,target=/restore \
  ubuntu \
  tar -xzf /backup/dorothy-data-backup.tar.gz -C /restore
```

Verify restored data:

`docker run --rm --mount source=dorothy-recovery,target=/restore,readonly ubuntu cat /restore/important.txt`

---

## 9. Important Docker Concepts

- Image = blueprint or template.
- Container = running or stopped instance of an image.
- Volume = Docker-managed persistent storage.
- Bind mount = connects a specific host folder to a container.
- Docker Desktop = graphical interface for managing Docker.
- Docker Engine = manages containers and images.

## 10. Host vs Container

**Ubuntu host terminal:** Docker management commands such as `docker ps`, `docker run`, `docker stop`, and `docker volume ls`.

**Inside a container:** Commands such as `pwd`, `ls`, `cat`, `whoami`, and `cd` operate on the container's environment.

**Important:** `exit` leaves an interactive container shell. It does not remove the container.

---

## 11. Practice Log

### 16 November 2026 — Docker Volumes and Backup Recovery

Completed:

- Created a named Docker volume called `dorothy-data`.
- Attached it to an Ubuntu container.
- Created `important.txt` inside the mounted volume.
- Removed the original container and confirmed the volume remained.
- Created another container and recovered the original file.
- Backed up the volume into a compressed `.tar.gz` archive.
- Created `dorothy-recovery`.
- Restored the archive into the recovery volume.
- Verified the restored file and its contents.

**Key lesson:** Containers can be replaced without losing data stored in a separate volume. Backups provide an additional layer of protection.

### Next learning objective

Dockerfiles and building custom Docker images.

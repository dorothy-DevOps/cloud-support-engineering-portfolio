# Cloud & DevOps Command Handbook

**Purpose:** A central reference for the commands, tools, and practical techniques I learn throughout my Cloud Architect and DevOps journey.

This handbook will grow progressively as I learn and practise. It will contain explanations and real examples, not just lists of commands.

## How to Use This Handbook

Every command entry should include:

- **Command:** Exact syntax.
- **Purpose:** What the command does.
- **Explanation:** How it works.
- **Where to run it:** Ubuntu/WSL2 host, inside a Docker container, Azure Cloud Shell, etc.
- **Practical example:** A real command I have practised.
- **Common mistakes:** Things to avoid.

## Table of Contents

1. Linux Fundamentals
2. Linux Administration
3. Linux Networking
4. Git and GitHub
5. Docker Fundamentals
6. Advanced Docker
7. Azure CLI
8. AWS CLI
9. Terraform
10. Kubernetes
11. CI/CD and GitHub Actions
12. Cloud and DevOps Security
13. Troubleshooting

---

# 1. Linux Fundamentals

*Commands for navigating the Linux filesystem and managing files and directories.*

### 1.1 Navigation and Directory Commands

#### `pwd`

- **Purpose:** Displays the current working directory.
- **Explanation:** Shows the full path of the directory you are currently working in.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
pwd
```

- **Expected result:** A directory path, such as `/home/dorothydev`.

#### `ls`

- **Purpose:** Lists files and directories.
- **Explanation:** Displays the contents of your current directory.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
ls
```

- **Useful variation:**

```bash
ls -la
```

Displays hidden files and detailed file information.

#### `cd`

- **Purpose:** Changes the current directory.
- **Explanation:** Moves you from one directory to another.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Examples:**

```bash
cd ~
cd /home/dorothydev
cd ..
```

- `cd ~` moves to your home directory.
- `cd ..` moves one directory level up.

#### `mkdir`

- **Purpose:** Creates a new directory.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
mkdir linux_practice
```

Creates a directory called `linux_practice`.

#### `touch`

- **Purpose:** Creates an empty file.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
touch daily-devops-notes.md
```

Creates an empty file if it does not already exist.

#### `cp`

- **Purpose:** Copies files or directories.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
cp notes.txt notes-backup.txt
```

Creates a copy of `notes.txt` named `notes-backup.txt`.

#### `mv`

- **Purpose:** Moves or renames files and directories.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
mv old-name.txt new-name.txt
```

Renames a file from `old-name.txt` to `new-name.txt`.


# 2. Linux Administration

*Commands for managing permissions, users, processes, and services.*

### 2.1 File Permissions and User Privileges

#### `chmod`

- **Purpose:** Changes file or directory permissions.
- **Explanation:** Controls who can read, write, or execute a file.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Examples:**

```bash
chmod 600 notes.txt
chmod 644 notes.txt
chmod 755 hello.sh
```

- `600`: Owner can read and write.
- `644`: Owner can read and write; others can read.
- `755`: Owner has full permissions; others can read and execute.

#### `chown`

- **Purpose:** Changes file or directory ownership.
- **Explanation:** Assigns ownership to a different user or group.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
sudo chown dorothydev:dorothydev notes.txt
```

Changes the owner and group of `notes.txt` to `dorothydev`.

#### `sudo`

- **Purpose:** Runs a command with elevated privileges.
- **Explanation:** Allows an authorised user to perform administrative tasks.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
sudo whoami
```

The output should be:

```text
root
```

### 2.2 Service Management

#### `systemctl`

- **Purpose:** Manages Linux services.
- **Explanation:** Checks service status, starts, stops, enables, disables, or restarts services.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Examples:**

```bash
systemctl is-active cron
systemctl is-enabled cron
sudo systemctl restart cron
```

#### `journalctl`

- **Purpose:** Views system and service logs.
- **Explanation:** Helps investigate service activity and troubleshoot errors.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
journalctl -u cron -n 20
```

Displays the latest 20 log entries for the cron service.

### 2.3 Process Management

#### `ps`

- **Purpose:** Displays information about running processes.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
ps aux
```

Displays a detailed list of running processes.

#### `top`

- **Purpose:** Monitors running processes and system resource usage.
- **Explanation:** Displays live information about CPU, memory, and processes.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
top
```

Press `q` to exit.

#### `kill`

- **Purpose:** Sends a signal to a process, commonly to stop it.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
kill 1234
```

Sends the default termination signal to process ID 1234. Replace `1234` with the actual process ID.


# 3. Linux Networking

*Commands for inspecting IP addresses, DNS, connectivity, and network services.*

### 3.1 IP Address and Network Information

#### `ip addr`

- **Purpose:** Displays network interfaces and their IP addresses.
- **Explanation:** Helps identify the IP addresses assigned to your machine.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
ip addr
```

Displays network interfaces such as `eth0` and `lo`.

### 3.2 Network Connectivity

#### `ping`

- **Purpose:** Tests connectivity between your machine and another host.
- **Explanation:** Sends ICMP echo requests and waits for replies.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
ping google.com
```

Tests whether your machine can reach Google and receive replies.

To limit the test to 10 packets:

```bash
ping -c 10 google.com
```

#### `curl`

- **Purpose:** Makes requests to web servers and retrieves information.
- **Explanation:** Can test whether a website or web service responds over HTTP or HTTPS.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
curl -I https://google.com
```

Displays HTTP response headers without downloading the full page.

### 3.3 DNS Troubleshooting

#### `nslookup`

- **Purpose:** Queries DNS servers to find DNS records.
- **Explanation:** Helps determine how domain names resolve to IP addresses or other domain names.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Examples:**

```bash
nslookup google.com
nslookup -type=A google.com
nslookup -type=CNAME www.github.com
```

- Default query: Displays DNS resolution information.
- `-type=A`: Queries the IPv4 address record.
- `-type=CNAME`: Queries the canonical name record.

### 3.4 Network Socket Inspection

#### `ss`

- **Purpose:** Displays information about network sockets.
- **Explanation:** Helps identify listening ports and network connections.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
ss -tuln
```

- `-t`: Displays TCP sockets.
- `-u`: Displays UDP sockets.
- `-l`: Displays listening sockets.
- `-n`: Displays numeric addresses and port numbers instead of resolving names.

# 4. Git and GitHub

*Commands for version control and collaborating through GitHub.*

### 4.1 Checking Git Installation and Configuration

#### `git --version`

- **Purpose:** Checks the installed Git version.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
git --version
```

#### `git config --global --list`

- **Purpose:** Displays global Git configuration.
- **Explanation:** Shows settings such as the configured username and email.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
git config --global --list
```

#### `git config --global user.name`

- **Purpose:** Sets the name Git associates with your commits.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
git config --global user.name "dorothy"
```

### 4.2 Checking Repository Status

#### `git status`

- **Purpose:** Shows the current state of a Git repository.
- **Explanation:** Identifies modified, untracked and staged files, and displays the current branch.
- **Where to run it:** Inside a Git repository, using the Ubuntu terminal.
- **Example:**

```bash
git status
```

### 4.3 Staging and Saving Changes

#### `git add`

- **Purpose:** Stages changes for the next commit.
- **Explanation:** Selects files to be included in a commit.
- **Where to run it:** Inside a Git repository.
- **Examples:**

```bash
git add README.md
git add .
```

The first stages one file. The second stages changes throughout the current directory and its subdirectories.

#### `git commit`

- **Purpose:** Saves staged changes as a Git commit.
- **Explanation:** Creates a recorded snapshot of the staged changes.
- **Where to run it:** Inside a Git repository.
- **Example:**

```bash
git commit -m "Update project documentation"
```

### 4.4 Sending Changes to GitHub

#### `git push`

- **Purpose:** Sends local commits to a remote repository.
- **Explanation:** Publishes committed changes to GitHub or another configured Git hosting service.
- **Where to run it:** Inside a Git repository.
- **Example:**

```bash
git push
```

### 4.5 Reviewing Changes

#### `git diff --stat`

- **Purpose:** Summarises changes in modified files.
- **Example:**

```bash
git diff --stat
```

#### `git diff -- README.md`

- **Purpose:** Displays line-by-line changes in a specific file.
- **Example:**

```bash
git diff -- README.md
```

#### `git diff --numstat`

- **Purpose:** Displays the number of lines added and removed.
- **Example:**

```bash
git diff --numstat
```

#### `git diff --ignore-space-at-eol --stat`

- **Purpose:** Shows a change summary while ignoring differences caused by whitespace at the ends of lines.
- **Example:**

```bash
git diff --ignore-space-at-eol --stat
```

#### `git ls-files --eol README.md`

- **Purpose:** Inspects the line-ending format of a tracked file.
- **Explanation:** Helps troubleshoot Windows CRLF and Linux LF line-ending differences.
- **Example:**

```bash
git ls-files --eol README.md
```


# 5. Docker Fundamentals

*Commands for downloading images, creating containers, inspecting container status, and managing container lifecycles.*


### 5.1 Working with Docker Containers

#### `docker ps`

- **Purpose:** Lists currently running containers.
- **Explanation:** Displays active containers, their IDs, images, status, and port mappings.
- **Where to run it:** Ubuntu terminal (WSL2 host), with Docker Desktop running.
- **Example:**

```bash
docker ps
```

**Important:** Stopped containers are not displayed.

#### `docker ps -a`

- **Purpose:** Lists all containers, including stopped containers.
- **Explanation:** The `-a` option means all containers.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker ps -a
```

Useful when checking containers that have exited.

### 5.2 Working with Docker Images

#### `docker images`

- **Purpose:** Lists Docker images stored locally.
- **Explanation:** Shows available images, their tags, image IDs, and sizes.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker images
```

**Important:** An image is a template used to create containers. Seeing an image does not mean a container is running.

#### `docker pull`

- **Purpose:** Downloads an image from a container registry.
- **Explanation:** Retrieves an image so Docker can use it to create containers.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker pull nginx
```

Downloads the Nginx image from Docker Hub.

### 5.3 Creating Containers

#### `docker run`

- **Purpose:** Creates and starts a new container.
- **Explanation:** Uses an image to create a container and starts it.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker run nginx
```

**Important:** Running this command again normally creates another container rather than restarting an existing one.

#### Running a named container with port mapping

```bash
docker run -d --name dorothy-nginx -p 8080:80 nginx
```

- `-d`: Runs the container in detached mode (background).
- `--name`: Assigns a name to the container.
- `-p 8080:80`: Maps host port 8080 to container port 80.
- `nginx`: Specifies the image.

### 5.4 Starting and Stopping Existing Containers

#### `docker start`

- **Purpose:** Starts an existing stopped container.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker start dorothy-nginx
```

Starts the existing container named `dorothy-nginx`.

#### `docker stop`

- **Purpose:** Stops a running container.
- **Explanation:** Gracefully stops the container without deleting it.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker stop dorothy-nginx
```

Stops the named container but keeps it available for later use.

#### `docker rm`

- **Purpose:** Removes a stopped container.
- **Explanation:** Deletes the container but does not delete the Docker image used to create it.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker rm dorothy-nginx
```

Removes the stopped container named `dorothy-nginx`.

**Important:** Stop a running container before removing it.

#### Safe container lifecycle sequence

```bash
docker run -d --name nginx-practice -p 8091:80 nginx
docker ps
docker stop nginx-practice
docker ps -a
docker rm nginx-practice
docker images
```

This sequence demonstrates creating, checking, stopping and removing a container, then confirming that the image remains available.

### 5.5 Viewing and Monitoring Container Logs

#### `docker logs`

- **Purpose:** Displays logs generated by a container.
- **Explanation:** Helps investigate application behaviour, startup messages, errors and requests.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker logs logs-practice
```

Displays the available logs from the `logs-practice` container.

#### `docker logs --tail`

- **Purpose:** Displays only the most recent specified number of log lines.
- **Explanation:** The `--tail` option limits the output, making it easier to focus on recent activity.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker logs --tail 10 logs-practice
```

Displays the last 10 lines of logs.

#### `docker logs -f`

- **Purpose:** Follows container logs in real time.
- **Explanation:** The `-f` option means follow. It displays new log entries as they are generated.
- **Where to run it:** Ubuntu terminal (WSL2 host).
- **Example:**

```bash
docker logs -f logs-practice
```

Useful when monitoring an application while testing it, such as refreshing a website and watching for new Nginx access log entries.

**Important:** Press `Ctrl + C` to stop following the logs. This does not stop the container.

#### Important log observations

- `200 OK` – The request was successful.
- `404 Not Found` – The requested resource could not be found.
- `500 Internal Server Error` – The server encountered an internal problem.

**Practical exercise:** Created an Nginx container, accessed its welcome page, viewed its logs, filtered the output to the last 10 lines, and followed new log entries while refreshing the website.

**Result:** Confirmed that Nginx served the homepage successfully with HTTP 200 and identified a missing favicon request returning HTTP 404.


### Docker Troubleshooting – Practical Commands

| No. | Command | Purpose | Where to Run |
|---|---|---|---|
| 1 | `docker ps` | Check running containers, status and published ports | Ubuntu/WSL host |
| 2 | `docker logs dorothy-nginx` | View container logs and investigate errors or failed requests | Ubuntu/WSL host |
| 3 | `docker inspect dorothy-nginx` | Examine detailed container configuration, status, IP address, network and port bindings | Ubuntu/WSL host |
| 4 | `docker exec -it dorothy-nginx sh` | Enter the running Nginx container | Ubuntu/WSL host |
| 5 | `ls -la /usr/share/nginx/html` | List website files and permissions | Inside container |
| 6 | `cat /etc/nginx/conf.d/default.conf` | Display Nginx configuration, including document root and index settings | Inside container |
| 7 | `cat /usr/share/nginx/html/index.html` | Display the website's HTML source code | Inside container |
| 8 | `exit` | Leave the container shell and return to Ubuntu/WSL | Inside container |
| 9 | `curl -I http://localhost:8080` | Test the website and display HTTP response headers | Ubuntu/WSL host |

#### Important concepts learned

**Container networking**
- Container IP: `172.17.0.2`
- Docker bridge gateway: `172.17.0.1`
- Host port: `8080`
- Container port: `80`
- Network mode: `bridge`

**Nginx website configuration**
- Document root: `/usr/share/nginx/html`
- Default index file: `index.html`
- Configuration file: `/etc/nginx/conf.d/default.conf`

**HTTP status codes**
- `200 OK` – Request successful.
- `404 Not Found` – Requested resource could not be found.
- `500 Internal Server Error` – Server encountered an internal problem.

#### Practical troubleshooting lessons

1. A running container does not automatically mean its application is working.
2. Container logs help identify startup issues and failed requests.
3. `docker inspect` reveals detailed container and networking information.
4. Nginx uses its document root to locate website files.
5. `curl -I` helps verify whether a website is responding successfully.
6. A 404 error means the server was reached, but the requested resource was not found. Investigate the website files, URL and Nginx configuration before restarting the container.

**Practical exercise:** Successfully investigated the `dorothy-nginx` container, examined its logs and configuration, inspected its website files, and verified its HTTP response.

**Result:** `HTTP/1.1 200 OK` – Nginx successfully served the website.


# 6. Advanced Docker

*Commands for Dockerfiles, image builds, volumes, bind mounts, and networks.*

# 7. Azure CLI

*Commands for managing Azure resources and infrastructure.*

# 8. AWS CLI

*Commands for interacting with AWS services.*

# 9. Terraform

*Commands for provisioning and managing infrastructure as code.*

# 10. Kubernetes

*Commands for managing clusters, workloads, services, and deployments.*

# 11. CI/CD and GitHub Actions

*Commands and techniques for automated building, testing, and deployment.*

# 12. Cloud and DevOps Security

*Commands and practices for identity, permissions, credentials, and secrets.*

# 13. Troubleshooting

*Diagnostic commands, common errors, and practical solutions.*
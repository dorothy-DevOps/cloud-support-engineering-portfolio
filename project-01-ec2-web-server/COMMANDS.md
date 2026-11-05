# Linux Commands Reference – Project 01

This document contains the Linux commands used during the deployment and configuration of the sample EduTech LMS homepage on an AWS EC2 instance running Amazon Linux 2023.

## 1. Basic Linux System Commands

| Command | Purpose | How it was used |
|---|---|---|
| `whoami` | Displays the current logged-in user. | Confirmed the EC2 login user was `ec2-user`. |
| `uname -a` | Displays system and kernel information. | Identified the Linux operating system and system architecture. |
| `pwd` | Displays the current working directory. | Confirmed the working directory was `/home/ec2-user`. |
| `ls -la` | Lists files and directories, including hidden files. | Inspected the contents of the home directory. |
| `df -h` | Displays filesystem disk usage in a readable format. | Checked available disk space on the EC2 instance. |

## 2. Installing and Managing Nginx

### Install Nginx

```bash
sudo dnf install nginx -y
```

**Purpose:** Installs the Nginx web server using the DNF package manager.

**How it was used:** Installed Nginx on the Amazon Linux 2023 EC2 instance.

### Start Nginx and enable it at boot

```bash
sudo systemctl enable --now nginx
```

**Purpose:** Starts the Nginx service immediately and configures it to start automatically when the server boots.

### Check Nginx service status

```bash
sudo systemctl status nginx
```

**Purpose:** Displays the current status of the Nginx service.

**Expected result:** `Active: active (running)`.

## 3. Working with Website Files

### Open the Nginx homepage file

```bash
sudo nano /usr/share/nginx/html/index.html
```

**Purpose:** Opens the default website HTML file using the Nano text editor.

**How it was used:** Replaced the default Nginx welcome page with a sample EduTech LMS homepage.

## 4. Testing the Web Server

### Test the website locally

```bash
curl http://localhost
```

**Purpose:** Sends an HTTP request to the local web server and displays the response.

**Expected result:** The HTML source code of the EduTech LMS homepage is displayed in the terminal.

**How it was used:** Verified that Nginx was serving the updated homepage locally from the EC2 instance.

## 5. Important Linux Concepts Learned

- `sudo`: Runs a command with elevated administrative privileges.
- `dnf`: Package manager used by Amazon Linux 2023.
- `systemctl`: Manages Linux system services.
- `nano`: Command-line text editor.
- `curl`: Command-line tool for making HTTP requests.
- `localhost`: Refers to the local machine, in this project, the EC2 instance.

## Project Outcome

Successfully installed and configured Nginx, deployed a sample EduTech LMS homepage, and verified the web server locally and through the EC2 instance's public IPv4 address.

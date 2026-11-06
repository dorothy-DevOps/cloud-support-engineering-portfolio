# Linux Commands Reference – Project 01

This document contains the Linux commands used during the deployment, configuration, monitoring and troubleshooting of the sample EduTech LMS homepage on an AWS EC2 instance running Amazon Linux 2023.

## 1. Basic Linux System Commands

| Command | Purpose | How it was used |
|---|---|---|
| `whoami` | Displays the current logged-in user. | Confirmed the EC2 login user was `ec2-user`. |
| `uname -a` | Displays system and kernel information. | Identified Linux system and architecture information. |
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

**Purpose:** Starts Nginx immediately and configures it to start automatically when the server boots.

### Check Nginx service status

```bash
sudo systemctl status nginx
```

**Purpose:** Displays the current status of the Nginx service.

**Expected result:** `Active: active (running)`.

### Check whether Nginx is active

```bash
sudo systemctl is-active nginx
```

**Purpose:** Checks whether the Nginx service is currently running.

### Check whether Nginx starts automatically at boot

```bash
sudo systemctl is-enabled nginx
```

**Purpose:** Confirms whether Nginx is enabled to start during system boot.

### Validate Nginx configuration

```bash
sudo nginx -t
```

**Purpose:** Checks the Nginx configuration for syntax errors before applying configuration changes.

**Expected result:** Syntax is OK and test is successful.

## 3. Working with Website Files

### Open the Nginx homepage file

```bash
sudo nano /usr/share/nginx/html/index.html
```

**Purpose:** Opens the website HTML file using the Nano text editor.

**How it was used:** Replaced the default Nginx welcome page with a sample EduTech LMS homepage.

## 4. Testing the Web Server

### Test the website locally

```bash
curl http://localhost
```

**Purpose:** Sends an HTTP request to the local web server and displays the response.

**Expected result:** The HTML source code of the EduTech LMS homepage is displayed in the terminal.

### Check HTTP response headers

```bash
curl -I http://localhost
```

**Purpose:** Checks the HTTP response headers without displaying the full webpage.

**Expected result:** `HTTP/1.1 200 OK` when the web server is responding successfully.

**How it was used:** Verified successful HTTP connectivity after Nginx recovery.

## 5. Nginx Log Inspection and Troubleshooting

### Inspect Nginx access logs

```bash
sudo tail -n 10 /var/log/nginx/access.log
```

**Purpose:** Displays the latest HTTP requests and response status codes recorded by Nginx.

**How it was used:** Identified requests that returned HTTP 404 errors.

### Inspect Nginx error logs

```bash
sudo tail -n 10 /var/log/nginx/error.log
```

**Purpose:** Displays recent Nginx errors to help investigate server problems.

### Follow access logs in real time

```bash
sudo tail -f /var/log/nginx/access.log
```

**Purpose:** Continuously displays new access log entries as requests reach the server.

### Follow error logs in real time

```bash
sudo tail -f /var/log/nginx/error.log
```

**Purpose:** Continuously monitors new Nginx error messages.

**How it was used:** Practised identifying missing webpage requests and interpreting HTTP 404 responses.

## 6. Nginx Service Failure and Recovery

### Stop Nginx

```bash
sudo systemctl stop nginx
```

**Purpose:** Stops the Nginx service.

**How it was used:** Simulated a controlled web server service failure during troubleshooting practice.

### Start Nginx

```bash
sudo systemctl start nginx
```

**Purpose:** Starts the Nginx service after a failure.

**How it was used:** Restored the web server after identifying that the Nginx service had stopped.

### Verify service recovery

```bash
sudo systemctl status nginx
```

**Purpose:** Confirms that Nginx has successfully restarted.

**Expected result:** `Active: active (running)`.

### Verify HTTP recovery

```bash
curl -I http://localhost
```

**Purpose:** Confirms that the web server is responding to HTTP requests after recovery.

**Expected result:** `HTTP/1.1 200 OK`.

**Important:** The `stop` command is included for learning and documentation. Only use it during an intentional, approved failure simulation because it interrupts website access.

## 7. Important Linux Concepts Learned

- `sudo`: Runs a command with elevated administrative privileges.
- `dnf`: Package manager used by Amazon Linux 2023.
- `systemctl`: Manages Linux system services.
- `nano`: Command-line text editor.
- `curl`: Command-line tool for making HTTP requests.
- `localhost`: Refers to the local machine, in this project, the EC2 instance.
- `tail`: Displays the end of a file, useful for inspecting logs.
- `HTTP 200`: Indicates a successful HTTP response.
- `HTTP 404`: Indicates that the requested resource was not found.

## 8. Project Outcome

Successfully installed and configured Nginx, deployed a sample EduTech LMS homepage, inspected web server logs, investigated an HTTP 404 error, simulated an Nginx service failure, restored the service and verified successful HTTP connectivity.

These exercises provided practical experience in Linux administration, Nginx monitoring, log analysis, root cause investigation and basic cloud support troubleshooting.
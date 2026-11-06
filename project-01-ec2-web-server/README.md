
# Project 01: AWS EC2 Web Server Deployment

## Project Overview
This project demonstrates how to deploy a basic web server on an AWS EC2 Linux instance.

## Project Objectives
- Launch and configure an EC2 instance.
- Understand AWS Security Groups.
- Connect to a Linux server.
- Install and configure Nginx.
- Host a simple LMS demonstration homepage.
- Test HTTP connectivity.
- Identify and troubleshoot common web server issues.

## Architecture
Laptop Browser → AWS Security Group → EC2 Linux Instance → Nginx Web Server

## AWS Services and Tools
- Amazon EC2
- Security Groups
- Linux
- Nginx
- SSH and HTTP

## Implementation Steps
1. Review AWS account billing and free-tier eligibility.
2. Select an AWS Region.
3. Launch an eligible EC2 Linux instance.
4. Configure the Security Group.
5. Connect to the instance.
6. Install Nginx.
7. Deploy a sample LMS homepage.
8. Test the website.
9. Document troubleshooting and lessons learned.

## Troubleshooting
This project includes practical troubleshooting exercises:

1. [Nginx 404 Error Troubleshooting](#phase-3-nginx-log-analysis-and-404-troubleshooting)
2. [Nginx Service Failure and Recovery](nginx-service-recovery.md)

## Evidence
All screenshots will be documented here.

## Project Status
In Progress

## Implementation Progress

### Phase 1: EC2 Web Server Deployment — Completed

**Tasks completed:**

- Launched an Amazon EC2 instance using Amazon Linux 2023.
- Configured Security Group rules for SSH and HTTP access.
- Connected to the Linux server using EC2 Instance Connect.
- Used Linux commands to inspect the server and check disk space.
- Installed Nginx using the DNF package manager.
- Enabled and started the Nginx service.
- Verified that Nginx was active and running.
- Tested the default Nginx welcome page using HTTP.

**Result:** Successfully deployed and accessed a working Nginx web server on AWS EC2.

**Status:** Phase 1 completed.

### Evidence

The following screenshots demonstrate the successful deployment:


1. [EC2 Instance Running](../../screenshots/01-ec2-instance-running.png)
2. [Successful Linux Server Login](../../screenshots/02-successful-server-login.png)
3. [Nginx Service Running](../../screenshots/03-nginx-running.png)
4. [Nginx Welcome Page](../../screenshots/04-nginx-welcome-page.png)
5. [EduTech LMS Homepage](../../screenshots/05-edutech-lms-homepage.png)
6. [Nginx 404 Error Page](../../screenshots/06-nginx-404-page.png)
7. [Nginx 404 Access Log](../../screenshots/07-nginx-404-access-log.png)
   
### Next Phase

Replace the default Nginx welcome page with a sample EduTech LMS homepage and verify external HTTP access.

## Phase 2: Deploying the EduTech LMS Homepage

### Objective
Replace the default Nginx welcome page with a sample Learning Management System (LMS) homepage hosted on the EC2 web server.

### Tasks Completed
- Accessed the EC2 Linux server using EC2 Instance Connect.
- Located the Nginx web root directory: `/usr/share/nginx/html/`.
- Opened and edited `index.html` using the Nano text editor.
- Created a sample EduTech LMS homepage using HTML.
- Saved the updated HTML file on the EC2 instance.
- Used `curl http://localhost` to verify the updated webpage locally.
- Accessed the updated LMS homepage through the EC2 instance's public IPv4 address.
- Refreshed the browser to display the latest version of the webpage.

### Result
Successfully replaced the default Nginx welcome page with a sample EduTech LMS homepage accessible through the EC2 instance's public IPv4 address.


### Phase 2 Status
Completed.

## Phase 3: Nginx Log Analysis and 404 Troubleshooting

### Objective
Investigate web server requests and diagnose a missing webpage using Nginx access and error logs.

### Troubleshooting Activities

1. Inspected the Nginx access log to identify incoming HTTP requests and response status codes.
2. Inspected the Nginx error log to investigate missing file errors.
3. Verified that the Nginx service was active using:

   ```bash
   sudo systemctl is-active nginx
   ```

4. Simulated a missing webpage by requesting `/test-page` through the browser.
5. Confirmed that the server returned an HTTP `404 Not Found` response.
6. Traced the failed request in the Nginx access log:

   ```text
   GET /test-page HTTP/1.1" 404
   ```

### Key Findings

- The EC2 instance was reachable.
- Nginx was running successfully.
- The LMS homepage remained accessible.
- The requested `/test-page` resource did not exist.
- Nginx returned HTTP 404, indicating a missing resource rather than a server outage.

### Troubleshooting Conclusion

The investigation confirmed that the web server was healthy. The 404 response occurred because the requested page was not available in the Nginx document root.

### Commands Used

```bash
sudo tail -n 10 /var/log/nginx/access.log
sudo tail -n 10 /var/log/nginx/error.log
sudo systemctl is-active nginx
sudo tail -n 5 /var/log/nginx/access.log
```

### Skills Practised

- Linux log inspection
- Nginx service monitoring
- HTTP status code interpretation
- Basic web server troubleshooting
- Identifying missing resources
- Evidence-based technical diagnosis

## Phase 4: Nginx Service Failure and Recovery

### Objective
Simulate an Nginx service failure, investigate the cause, restore the service, and verify recovery.

### Troubleshooting Activities
- Checked the EC2 instance health and AWS status checks.
- Verified the Nginx service status.
- Simulated a service failure by stopping Nginx.
- Confirmed that HTTP requests failed while the service was stopped.
- Investigated the root cause.
- Restarted Nginx and verified that the service was active.
- Used curl to confirm a successful HTTP 200 OK response.

### Root Cause
The Nginx service had been stopped and was no longer listening for HTTP requests on port 80.

### Resolution
Restarted the Nginx service and confirmed successful HTTP connectivity.

### Detailed Incident Report
[View Nginx Service Recovery Report](nginx-service-recovery.md)

### Evidence
- [Nginx Service Restored](Screenshots/nginx-service-restored.png)
- [HTTP 200 Recovery Verification](Screenshots/nginx-http-200-recovery.png)

### Skills Practised
- Linux service management
- Nginx troubleshooting
- Root cause analysis
- HTTP connectivity testing
- Service recovery and verification

### Phase 4 Status
Completed.




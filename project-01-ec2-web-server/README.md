
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
Issues encountered and their solutions will be documented here.

## Evidence
### Evidence

The following screenshots demonstrate the successful deployment:

1. [EC2 Instance Running](screenshots/01-ec2-instance-running.png)
2. [Successful Linux Server Login](screenshots/02-successful-server-login.png)
3. [Nginx Service Running](screenshots/03-nginx-running.png)
4. [Nginx Welcome Page](screenshots/04-nginx-welcome-page.png).

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

Screenshots documenting the deployment are stored in the `screenshots` folder.

### Next Phase

Replace the default Nginx welcome page with a sample EduTech LMS homepage and verify external HTTP access.





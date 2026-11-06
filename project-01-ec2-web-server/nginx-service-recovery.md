# Nginx Service Failure and Recovery

## 1. Project Overview
**Project:** EC2 Web Server Deployment  
**Exercise:** Nginx Service Failure and Recovery  
**Environment:** AWS EC2 (Linux)  
**Web Server:** Nginx

## 2. Incident Description
A simulated service outage was introduced by deliberately stopping the Nginx service on the EC2 instance.

The objective was to investigate the service failure, identify the cause, restore the web server, and verify service recovery.

## 3. Initial Health Checks

| Check | Result |
|---|---|
| EC2 instance state | Running |
| AWS status checks | 3/3 passed |
| Nginx service | Active |
| Nginx startup configuration | Enabled |
| Nginx configuration test | Syntax OK |
| Initial HTTP response | 200 OK |

## 4. Failure Simulation

Command used:

```bash
sudo systemctl stop nginx
```

Service verification:

```bash
sudo systemctl status nginx
```

**Observed result:** Nginx was inactive.

Website connectivity test:

```bash
curl -I http://localhost
```

**Observed result:** Failed to connect.

## 5. Root Cause Analysis

The Nginx service was stopped and was therefore unable to accept HTTP requests on port 80.

This caused the local website connectivity test to fail.

## 6. Recovery Procedure

Nginx was restarted using:

```bash
sudo systemctl start nginx
```

Service status was checked again and confirmed as active.

## 7. Recovery Verification

The website was tested again:

```bash
curl -I http://localhost
```

**Observed result:** HTTP 200 OK.

The successful response confirmed that Nginx had resumed serving HTTP requests.

## 8. Key Skills Practised

- Linux service management using systemctl.
- Nginx service troubleshooting.
- HTTP connectivity testing using curl.
- Identifying service availability issues.
- Restoring a stopped web service.
- Verifying service recovery.

## 9. Conclusion

The simulated Nginx service outage was successfully investigated and resolved.

The web server was restored, and the final HTTP test returned 200 OK, confirming successful recovery.
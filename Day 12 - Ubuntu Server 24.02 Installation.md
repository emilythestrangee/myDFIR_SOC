## Introduction

Goal: Set up an SSH server in the cloud and view authentication logs in real time.

---

## Set Up Ubuntu Server

Deploy a new Ubuntu VM on vultr.com:

- Select **Cloud Compute** and a **Shared CPU** configuration.
- Choose Ubuntu 24.04 LTS as the OS image.
- Set CPU and memory (e.g., 1 vCPU, 1 GB RAM) and adjust storage to 25 GB.
- In **Networking/Advanced Options**, attach the VM to your VPC and reserve an external IP.
- Add a tag (e.g., `vuln-linux`) for firewall rules configuration.
- Disable IPv6 and auto-backups.
- Click **Create** and wait for the VM to start.

Once the VM shows as **Running**, connect via SSH.

---

## Access the Server

Use a terminal or PowerShell session to SSH into the server:

`ssh root@<IP_address>`

Accept the connection and enter the password if prompted.

---

## Update and Upgrade Repositories

Ensure the system is up-to-date by running:

`apt-get update && apt-get upgrade -y`

---

## Review Authentication Logs

Authentication activity is recorded in `/var/log/auth.log`.

Navigate to the logs directory:

`cd /var/log ls`

View the authentication log:

`cat auth.log`

---

## Filter Authentication Logs

To monitor failed login attempts or privileged user activity:

- Filter failed attempts:
    

`grep -i failed auth.log`

- Filter failed attempts for the root user:
    

`grep -i failed auth.log | grep -i root`

- Extract source IP addresses of failed attempts:
    

`grep -i failed auth.log | grep -i root | cut -d ' ' -f 11`

---

## Optional: Generate Test Logs

If no failed login attempts are present, simulate attempts from another machine **without authorized public keys** to generate authentication logs. This helps verify that logging is working correctly.

---

## Firewall Configuration (Optional)

To observe login attempts from external sources:

- Create a firewall rule to allow SSH (TCP 22) from all IPs:
    
    - Target tags: `vuln-linux`
        
    - Source IP range: `0.0.0.0/0`
        
    - Name: `allow-ssh-vuln-linux`
        

This can help generate logs for testing brute-force detection later.

---

## Summary

- Ubuntu 24.04 VM is deployed and updated.
    
- SSH access is configured.
    
- Authentication logs are available in `/var/log/auth.log`.
    
- Failed login attempts can be filtered and analyzed.
    
- VM is ready for Elastic Agent installation to forward logs to Kibana.

**Reference**
https://youtu.be/qsMhmXIqWfc?si=5nRfnmSaObEIWw0A

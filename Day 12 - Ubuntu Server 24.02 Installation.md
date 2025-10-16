## Introduction

Goal: Set up an SSH server in the cloud and view authentication logs in real time.

---

## Set Up Ubuntu Server

Deploy a new Ubuntu VM on vultr.com:

- Select **Cloud Compute** and a **Shared CPU** configuration.
- Choose Ubuntu 24.04 LTS as the OS image.
- Set CPU and memory (e.g., 1 vCPU, 1 GB RAM) and adjust storage to 25 GB.
- Disable IPv6 and auto-backups.
- Hostname: MYDFIR-Linux-emaan
- Click **Deploy Now** and wait for the VM to start.

Once the VM shows as **Running**, connect via SSH.

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
<img width="689" height="263" alt="image" src="https://github.com/user-attachments/assets/6ea4ba90-1ef0-4882-b42e-c8ea733e634c" />

---

## Filter Authentication Logs

To monitor failed login attempts or privileged user activity:

- Filter failed attempts:
`grep -i failed auth.log`
<img width="698" height="362" alt="image" src="https://github.com/user-attachments/assets/edadff3e-19f2-49bf-a2b5-dfe7b194dccc" />


- Filter failed attempts for the root user:
`grep -i failed auth.log | grep -i root`

<img width="613" height="68" alt="image" src="https://github.com/user-attachments/assets/bc7c9895-bd12-4bd7-a1f0-e6fac2212bd0" />
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

## 1. Introduction

**Goal**: We’ll deploy a new Ubuntu server to host our Mythic C2 instance and configure a secure environment to ensure only authorized machines can access it.

---

## Deploying the Mythic Server

### 2.1 Cloud Deployment

- Use vultr.com to create a new VM.
- **Cloud Compute** (Shared CPU)
- Minimum recommended specs: **2 CPU cores** and **4 GB RAM**.
- OS: Ubuntu 22.04.
- Hostname: MYDFIR-MYTHIC
- Deploy the server.

---

## 3. Firewall Configuration

To secure the Mythic instance:

- Allow ingress traffic from:
    - Personal IP (your attacker machine).
    - IPs of the target servers (Windows Server and Linux Server).
- Deny all other inbound traffic.
- Optional: Add IAP rule if using GCP SSH web service.

Example rules:

- **allow-mypc-mythic** → Source: Personal IP, allow all ports.
- **allow-targets-mythic** → Source: target VM tags.

---

## 4. Installing Mythic C2

### 4.1 System Update

`apt-get update && apt-get upgrade -y`

### 4.2 Install Dependencies

`apt install docker-compose apt install make`

Press enter or click to view image in full size

### 4.3 Clone Mythic Repository

`git clone https://github.com/its-a-feature/Mythic cd Mythic`

### 4.4 Run Installation Script

`./install_docker_ubuntu.sh`

---

## 5. Docker Troubleshooting

If Docker fails to start:

`systemctl status docker systemctl restart docker`


---

## 6. Start Mythic

Within the Mythic directory:

`make ./mythic-cli start`

Once it’s running, access the web interface:

`https://<mythic-server-ip>:7443`


---

## 7. Accessing Credentials

Mythic’s default credentials are stored in the hidden `.env` file:

`ls -la cat .env`

Look for:

- `MYTHIC_ADMIN_USER`
- `MYTHIC_ADMIN_PASSWORD`

---

## 8. Firewall Hardening

- Only allow TCP traffic on all ports from:
    - Personal IP
    - Target Windows and Linux servers
- Block all other sources.

---

## 9. Accessing the Dashboard

- URL: `https://<publicIP>:7443`
- Login with the credentials from the `.env` file.
- Explore the interface to familiarize yourself with:
    
    - **Callbacks** (connected agents)
    - **Payloads**
    - **File Hosting**
    - **Artifacts**
    - **MITRE ATT&CK Mapping**
    - **Dark Mode**

---

## 10. Summary

- Deployed and configured a Mythic C2 server.
- Installed required dependencies.
- Secured access with firewall rules.
- Accessed the Mythic dashboard successfully.

**Reference**
https://youtu.be/JKO1pZ45_5I?si=d1IJH0__AzCTxdOi
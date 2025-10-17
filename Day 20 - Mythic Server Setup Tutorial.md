## Introduction

Goal: setting up **Mythic**, a Command and Control (C2) framework used for red teaming operations. For this setup, we will use **Vultr** as our cloud provider to host the Mythic C2 instance.  **Kali Linux** will also be installed to serve as the attacker machine.

---

## Deploying a Server on Vultr

1. Log in to Vultr and click **“Deploy New Server.”**
2. Select the following options:
    
    - Cloud Compute (Shared CPU)
    - Location: Singapore
    - OS: Ubuntu (4 GB RAM)
    - Disable Auto Backups & IPv6
    - Set Hostname: `MYDFIR-MYTHIC`

3. Deploy the server.

---

## Installing Kali Linux

1. Download Kali Linux from the official website.
2. Choose the Virtual Machine version (VMware is used in this setup).
3. Extract the downloaded file using 7-Zip.
4. Locate and open the `.vmx` file in **VMware Workstation**.
5. Power on the Kali VM.

---

## Setting Up Mythic C2

### Accessing the Vultr Instance (MYDFIR-MYTHIC)

`ssh root@<MYDFIR-MYTHIC_ip>`

### Update & Upgrade the System

`apt-get update && apt-get upgrade -y`

### Install Dependencies

`apt-get install docker-compose 
``
### Clone & Install Mythic

`git clone https://github.com/its-a-feature/Mythic.git cd Mythic ./install_docker_ubuntu.sh`

### Start Mythic

`make ./mythic-cli start`

Press enter or click to view image in full size

---

## Fixing Docker Issues

If Docker fails to start, use:

`systemctl restart docker`

Verify the status:

`systemctl status docker`

---

## Configuring Firewall

Create a firewall group `Challenge-Mythic-Firewall` on Vultr.

Allow TCP traffic from trusted IPs only:

- TCP 1–65535 (Source: Personal IP)
    
- TCP 1–65535 (Source: Challenge-WIN-Haji)
    
- TCP 1–65535 (Source: Challenge-Linux-Steve)
    

Apply `Challenge-Mythic-Firewall` to the Mythic server.

Press enter or click to view image in full size

---

## Accessing Mythic C2 Web Interface

1. Copy the server’s public IP.
    
2. Open a browser and navigate to:
    
    `https://<mythic-server_ip>:7443`
    

### Login Credentials

- Default Username: `mythic_admin`
    
- Password: Stored in the `.env` file
    

`cat .env`

You can also change the default operation name:

- Default: `operation-cima`
    
- Example new name: `operation-challenge`
    

Press enter or click to view image in full size

---

## Mythic C2 Dashboard Overview

- **Callbacks:** List of connected agents.
    
- **Payloads:** Preconfigured exploits.
    
- **File Hosting:** Upload and download files.
    
- **Artifacts:** Keylogging, screenshots, credentials, reports.
    
- **MITRE ATT&CK Mapping:** Identify and map attack techniques.
    
- **Tags:** Categorize and organize targets.
    
- **Dark Mode:** Available for UI preference.
    

---

## Conclusion

We successfully deployed a Mythic C2 instance on Vultr, configured the firewall, and accessed the Mythic dashboard.  
This setup will be used in the next sessions to generate payloads from Kali Linux and launch controlled attacks against the Windows Server target.

**Reference**
https://youtu.be/JKO1pZ45_5I?si=d1IJH0__AzCTxdOi
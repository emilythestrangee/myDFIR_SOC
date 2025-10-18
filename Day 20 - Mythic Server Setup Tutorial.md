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
`apt install make`

### Clone & Install Mythic

`git clone https://github.com/its-a-feature/Mythic.git
`cd Mythic 
`./install_docker_ubuntu.sh`

<img width="748" height="263" alt="image" src="https://github.com/user-attachments/assets/23534453-bcf4-49cf-bd6b-d2dfe387f19f" />


<img width="711" height="352" alt="image" src="https://github.com/user-attachments/assets/d6abf821-df82-440d-b4bf-2c629b73bc46" />



### Start Mythic

`make 
`./mythic-cli start`

On running the "make" command, an error occurs that suggests issues with Docker daemon. "systemctl status docker" shows that the service failed.

## Fixing Docker Issues

If Docker fails to start, use:
`systemctl restart docker`

Verify the status:
`systemctl status docker`

<img width="388" height="232" alt="image" src="https://github.com/user-attachments/assets/6eec2e98-709f-4773-9b3f-e0f8cec07822" />

<img width="583" height="326" alt="image" src="https://github.com/user-attachments/assets/18021245-cdbe-482f-aec1-9f44396da736" />



---
## Configuring Firewall

Create a firewall group `MyDFIR-Mythic-Firewall` on Vultr.

Allow TCP traffic from trusted IPs only:

- TCP 1–65535 (Source: Personal IP)
- TCP 1–65535 (Source: MYDFIR-WIN-emaan)
- TCP 1–65535 (Source: MYDFIR-Linux-emaan)

Apply `MyDFIR-Mythic-Firewall` to the Mythic server and update firewall group.
<img width="531" height="406" alt="image" src="https://github.com/user-attachments/assets/2408fc16-5972-458f-ba2a-1717c15d0314" />


---

## Accessing Mythic C2 Web Interface

1. Copy the server’s public IP.
2. Open a browser and navigate to:
    
    `https://<MYDFIR-MYTHIC_ip>:7443`
    

### Login Credentials

- Default Username: `mythic_admin`
- Password: Stored in the `.env` file

`cat .env`
<img width="864" height="848" alt="image" src="https://github.com/user-attachments/assets/c18df90c-ac8d-40ff-8902-6959889bbafb" />

You can also change the default operation name:

- Default: `operation-cima`
- Example new name: `operation-challenge`
<img width="906" height="676" alt="image" src="https://github.com/user-attachments/assets/3177bc82-a61f-4474-8954-e16738a99c53" />



<img width="1100" height="541" alt="image" src="https://github.com/user-attachments/assets/46b4a6d6-5fd2-4122-90f9-4097a713a50f" />



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

## Introduction

Goal: Deploy and configure an **Elasticsearch** instance in the cloud using **Vultr**, and prepare it for SOC operations. This includes creating a **Virtual Private Cloud (VPC)**, deploying a **VM**, installing Elasticsearch, configuring network access, and securing it with a firewall.

---

## 1. Creating a Virtual Private Cloud (VPC)

1. Sign up on **Vultr** and navigate to: **Products → Network → VPC 2.0 → Add VPC**.

    
2. Configure your network:
    
    - **Location:** Pick a data center region where all VMs will reside.
        
    - **IPv4 Range:** `172.31.0.0/24`
        
    - **Name:** MyDFIR-SOC-Challenge
        
3. Click **Add Network**.
    
![[Pasted image 20251016175921.png]]
> **Note:** All VMs created must be under this VPC for internal communication.

---

## 2. Deploying a Virtual Machine (VM)

1. Go to **Deploy → Deploy New Server**.
    
2. Configure the VM:
    
    - **Location:** Same as the VPC.
        
    - **Image:** Ubuntu 22.04 x64
        
    - **Plan:** 4 vCPUs, 80GB storage
        
    - **Additional Features:** Disable auto-backups and IPv6.
        
    - **VPC:** Select the VPC you just created.
        
    - **Hostname:**  MyDFIR-ELK
        
3. Click **Deploy** and wait until the status is **Running**.
    


---

## 3. Connecting to the VM

Use **SSH** to connect from your local machine:

`ssh root@<public-IP>`

- Accept the connection and log in with the provided password.
    
- Update and upgrade the VM:
    

`apt-get update && apt-get upgrade -y`

_img: placeholder for SSH session screenshot_

---

## 4. Installing Elasticsearch

1. Download the Elasticsearch Debian package:
    

`wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.17.2-amd64.deb`

2. Install it:
    

`dpkg -i elasticsearch-8.17.2-amd64.deb`

3. **Important:** Save the **Security Auto-Configuration Information**, which contains the `elastic` superuser credentials.
    
4. If needed, reset the superuser password:
    

`/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic`

_img: placeholder for download/install screenshot_

---

## 5. Configuring Elasticsearch

1. Open the configuration file:
    

`nano /etc/elasticsearch/elasticsearch.yml`

2. Update network settings to allow external access:
    
    `network.host: <public-IP> http.port: 9200`
    
3. Save and exit (`Ctrl + X`, then `Y`, then `Enter`).
    

_img: placeholder for elasticsearch.yml screenshot_

---

## 6. Setting Up a Firewall

1. In Vultr, navigate to: **VM Settings → Firewall → Manage → Add Firewall Group**.
    
2. Configure the firewall:
    
    - Name: `SOC-Simulation`
        
    - Source: Restrict SSH and Elasticsearch ports (22, 9200) to your IP only.
        
3. Assign the firewall to your VM: **Compute → VM → Settings → Firewall → Update Firewall Group**.
    

_img: placeholder for firewall configuration screenshot_

---

## 7. Starting Elasticsearch Service

Run the following commands on the VM:

`systemctl daemon-reload systemctl enable elasticsearch.service systemctl start elasticsearch.service systemctl status elasticsearch.service`

- If the service shows **dead**, run:
    

`systemctl start elasticsearch.service`

- Verify the service is **active** and running.
    

_img: placeholder for Elasticsearch service status screenshot_

---

## 8. Accessing Elasticsearch

- Open a browser and navigate to:
    

`https://<external-IP>:9200`

- Log in with the `elastic` user and the password from the Security Auto-Configuration information.
    

_img: placeholder for Elasticsearch login screenshot_

---

## Conclusion

By the end of this setup:

- You created a **VPC** on Vultr for secure internal networking.
    
- Deployed and connected to a **Ubuntu VM**.
    
- Installed and configured **Elasticsearch**, making it accessible externally.
    
- Set up **firewall rules** to restrict access only to authorized IPs.
    

This completes the foundational setup for a SOC environment using ELK.

_img: placeholder for full system architecture diagram_


**Reference**
https://youtu.be/ypXARA5Uk4I?si=5sEwz--V8NcvFry8
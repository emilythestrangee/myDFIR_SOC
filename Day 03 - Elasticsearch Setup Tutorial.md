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
   <img width="270" height="337" alt="image" src="https://github.com/user-attachments/assets/47649c1d-2b7d-453c-be35-a2d99a69f4a0" />



---

## 3. Connecting to the VM

Use **SSH** to connect from your local machine:

`ssh root@207.148.79.228>`

- Accept the connection and log in with the provided password.
    
- Update and upgrade the VM:
`apt-get update && apt-get upgrade -y`

<img width="719" height="178" alt="image" src="https://github.com/user-attachments/assets/303f7a31-3626-4e71-a98c-cc2652e62adb" />


---

## 4. Installing Elasticsearch

1. Download the Elasticsearch Debian package:
`wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-9.1.5-amd64.deb'

2. Install it:
`dpkg -i elasticsearch-9.1.5-amd64.deb`

3. **Important:** Save the **Security Auto-Configuration Information**, which contains the `elastic` superuser credentials.
    
4. If needed, reset the superuser password:
`/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic`

---

## 5. Configuring Elasticsearch

1. Open the configuration file:
`nano /etc/elasticsearch/elasticsearch.yml`

2. Update network settings to allow external access, start by removing the comments (#):
    
    `network.host: 207.148.79.228 http.port: 9200`
    
2. Save and exit (`Ctrl + X`, then `Y`, then `Enter`).

<img width="703" height="276" alt="image" src="https://github.com/user-attachments/assets/7e7bcdb3-3bb8-422f-909b-3a5c0c30f65c" />


## 6. Setting Up a Firewall

1. In Vultr, navigate to: **VM Settings → Firewall → Manage → Add Firewall Group**.
    
2. Configure the firewall:
    
    - Name: MyDFIR-SOC-Challenge
        
    - Source: Restrict SSH port (22) to your IP only.
    
3. Assign the firewall to your VM: **Compute → VM → Settings → Firewall → Update Firewall Group**.
   <img width="1100" height="361" alt="image" src="https://github.com/user-attachments/assets/dd8ced67-4b21-431c-b3d7-7a6b04e52014" />


<img width="530" height="360" alt="image" src="https://github.com/user-attachments/assets/9375bfa3-bd72-4b5b-83ca-d83f975df6e3" />


## 7. Starting Elasticsearch Service

Run the following commands:

`systemctl daemon-reload systemctl enable elasticsearch.service systemctl start elasticsearch.service systemctl status elasticsearch.service`

- If the service shows **dead**, run:
`systemctl start elasticsearch.service`

- Verify the service is **active** and running.
    
<img width="671" height="137" alt="image" src="https://github.com/user-attachments/assets/162b47e6-2e67-4afb-9e66-443a2c3a3230" />


---


## Conclusion

By the end of this setup:

- You created a **VPC** on Vultr for secure internal networking.
    
- Deployed and connected to a **Ubuntu VM**.
    
- Installed and configured **Elasticsearch**, making it accessible externally.
    
- Set up **firewall rules** to restrict access only to authorized IPs.
    


**Reference**
https://youtu.be/ypXARA5Uk4I?si=5sEwz--V8NcvFry8

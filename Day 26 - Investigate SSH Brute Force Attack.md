### 1. Introduction

**Goal:** Learn how to investigate a potential DNS exfiltration alert and identify suspicious patterns that indicate data leakage through DNS queries.

---

### 2. Accessing Alerts

**Steps:**

- **Navigate to Alerts:**
    
    - Click on the hamburger icon and select **“Alerts”** under the security tab.
    - Review the list of alerts and select the one related to **DNS exfiltration**.

![[Pasted image 20251018234405.png]]
- **View Alert Details:**
    
    - Expand the alert by clicking on **“View Details.”**
    - Review the rule name, detection source, and description of the alert.
    - For instance, check for the `MyDFIR-SSH Brute Force Activity-emaan` alert.
    - After checking a few alerts, a certain ip stood out i.e., **183.66.17.82**.

---

### 3. Investigative Methodology

**Key Questions:**

1. **Is the IP Known for Brute Force Activity?**:
    
    - Use tools like AbuseIPDB and GreyNoise to check the reputation of the IP address.
    - Example: An IP reported 1,816 times with 100% confidence of abuse is highly suspicious.

![[Pasted image 20251019000458.png]]

2. **Were Any Other Users Affected by This IP?**:
    
    - Search for the IP address in Kibana's Discover tab.
    - Check for distinct users targeted by the IP.
    - Example: Users root, oracle, guest, and test were targeted.
    - Some reports were made on AbuseIPDB about this ip regarding SSH brute force attacks.
![[Pasted image 20251019000649.png]]


3. **Were Any Logins Successful?**:
    
    - Search for successful login attempts using keywords like `accepted`.
    - Ensure correct capitalization in queries.
    - No successful logins found within the last 30 days.

4. **What Activity Occurred After Successful Logins?**:
    
    - If there were successful logins, investigate subsequent activities.
    - Look for signs of malicious behavior like downloading scripts, running discovery commands, or executing malicious files.

---

### 4. Closing the Alert

**Steps:**

- **Document Findings:**
    
    - Record details of suspicious domains, source IPs, and query patterns.
        
    - Add relevant screenshots or log excerpts in the incident notes.
        
- **Block or Contain:**
    
    - If malicious activity is confirmed, block the domain at the DNS firewall.
        
    - Isolate the affected host for further forensic investigation.
        
- **Update Detection Rules:**
    
    - Modify or fine-tune DNS exfiltration detection rules to catch similar behavior faster.
        

---

### 5. Integrating with Ticketing System

**Steps:**

- **Configure Alert Routing:**
    
    - Under “Rules,” select the DNS exfiltration detection rule.
        
    - Edit the rule to push alerts via webhook to the ticketing platform.
        
- **Customize Ticket Details:**
    
    - Include relevant variables such as domain, source IP, and rule name.
        
    - Remove unnecessary fields like attachments if not needed.
        
- **Test Integration:**
    
    - Save the rule configuration.
        
    - Run a test to ensure tickets are being created correctly.
        

---

### 6. Reviewing and Closing Tickets

**Steps:**

- **Review Ticket:**
    
    - Check that the ticket contains alert details and investigation notes.
        
    - Ensure the responsible analyst is assigned.
        
- **Document Resolution:**
    
    - Add remediation steps such as domain blocking or host isolation.
        
    - Reference any detection rule changes.
        
- **Close Ticket:**
    
    - Provide a closing summary and select **Resolved**.
        
    - Ensure all logs and notes are attached to support future investigations.

**Reference**
https://youtu.be/sXQ1hsAFX7U?si=Uw3wS9MFCwbo62J1

### 1. Introduction

**Objective:** Learn how to identify and investigate RDP brute force attempts, trace the source, and understand the scope of potential compromise.

---

### 2. Accessing and Reviewing Alerts

**Steps:**

- **Locate the Alert:**
    
    - Navigate to **Security → Alerts**.
    - Filter for alerts related to **RDP login failures** or brute force detection.
    - Select the alert you want to investigate.
    
- **Review Alert Details:**
    
    - Expand the alert to see metadata such as the **source IP, target username, timestamp, and number of failed attempts**.
    - Take note of any contextual information provided by the detection rule.


---

### 3. Investigation Strategy

**Key Questions:**

- **Is the Source IP Malicious?**
    
    - Check the IP against threat intelligence feeds like AbuseIPDB, GreyNoise, or VirusTotal.
    - Determine if the IP has a history of brute force activity.

- **Which Accounts Were Targeted?**
    
    - Use the SIEM to search for all login attempts from that IP.
    - Identify all users that were targeted to understand the scope.
    
- **Were Any Logins Successful?**
    
    - Look for successful RDP logins using event codes such as **4624** (Windows Logon Success).
    - Pay attention to the session type and logon IDs for tracking post-login activity.

- **Post-Login Activity:**
    
    - If successful logins are detected, investigate actions performed during the session.
    - Check for unusual process creation, script execution, privilege escalation, or lateral movement attempts.

---

### 4. Investigating Post-Login Activity

**Steps:**

- **Normalize Time Zone:**
    
    - In Kibana Stack Management → Advanced Settings, set the timezone to UTC for consistency.
        
- **Track Session Using Logon ID:**
    
    - Expand event details to find the **logon ID**.
    - Filter logs by this ID to monitor all actions performed in that session.
        
- **Analyze Events:**
    
    - Check for privilege changes or elevated permissions.
    - Monitor for process creation, file access, network connections, or attempts to move laterally.
        
- **Document Findings:**
    
    - Record the start and end times of the session.
    - Note any malicious or unusual activity observed, such as persistence mechanisms, data exfiltration attempts, or C2 connections.
        

---

### 5. Closing the Investigation

**Steps:**

- Document all findings in the ticketing system.
    
- If malicious activity is confirmed:
    
    - Block the source IP.
    - Enforce account lockouts or password resets.
    - Update RDP brute force detection rules if needed.

- Close the alert once mitigation and documentation are complete.

### 2. Accessing Alerts

**Steps:**

- **Navigate to Alerts:**
    
    - Click on the hamburger icon and select **“Alerts”** under the security tab.
    - Review the list of alerts and select one for analysis.



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




2. **Were Any Other Users Affected by This IP?**:
    
    - Search for the IP address in Kibana's Discover tab (discovered some targeted users).
    - Check for distinct users targeted by the IP.
    - Example: Users root, oracle, guest, and test were targeted.
    - Some reports were made on AbuseIPDB about this ip regarding SSH brute force attacks.






3. **Were Any Logins Successful?**:
    
    - Search for successful login attempts using keywords like `accepted`.
    - Ensure correct capitalization in queries.
    - No successful logins found found.



4. **What Activity Occurred After Successful Logins?**:
    
    - If there were successful logins, investigate subsequent activities.
    - Look for signs of malicious behavior like downloading scripts, running discovery commands, executing malicious files or performing persistence.
    - Once investigation is finished we can either close the alert or escalate it depending on our findings.

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

**Steps to configure Elastic instance to push alerts to osTicket so we can automatically generate a ticket on osTicket for each alert generated:**

- **Configure Alert Routing:**
    
    - Under “Rules,” modify our existing rule for SSH brute force attacks and add an “Action”.



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
https://youtu.be/l9KA6dPdOs8?si=w4SZCr8RGoZv7R1K
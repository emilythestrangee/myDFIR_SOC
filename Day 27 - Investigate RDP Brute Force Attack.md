
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
    -  For instance, check for the `MyDFIR-RDP Brute Force Activity-emaan` alert.
    - After checking a few alerts, a certain ip stood out i.e., **91.238.181.8**.


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




### 6. Integrating with Ticketing System

**Steps to configure Elastic instance to push alerts to osTicket so we can automatically generate a ticket on osTicket for each alert generated:**

- **Configure Alert Routing:**
    
    - Under “Rules,” modify our existing rule for RDP brute force attacks and add an “Action”.


- **Customize Ticket Details:**
    
    - Include relevant variables such as domain, source IP, and rule name.
    - Remove unnecessary fields like attachments if not needed.

- **Test Integration:**
    
    - Save the rule configuration.
    - Run a test to ensure tickets are being created correctly.

**Reference**
https://youtu.be/l9KA6dPdOs8?si=w4SZCr8RGoZv7R1K
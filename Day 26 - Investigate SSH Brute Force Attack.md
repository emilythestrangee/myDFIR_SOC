### 1. Introduction

**Goal:** Learn how to investigate a potential SSH brute force activity alert and identify suspicious patterns that indicate SSH brute force attempts.

---

### 2. Accessing Alerts

**Steps:**

- **Navigate to Alerts:**
    
    - Click on the hamburger icon and select **“Alerts”** under the security tab.
    - Review the list of alerts and select one for analysis.

<img width="616" height="542" alt="image" src="https://github.com/user-attachments/assets/6b009d49-64ae-4a99-9c87-b978bb5b4a32" />

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

<img width="892" height="692" alt="image" src="https://github.com/user-attachments/assets/722d82e8-701f-4fbb-ab2b-d3033e95d0eb" />


2. **Were Any Other Users Affected by This IP?**:
    
    - Search for the IP address in Kibana's Discover tab (discovered some targeted users).
    - Check for distinct users targeted by the IP.
    - Example: Users root, oracle, guest, and test were targeted.
    - Some reports were made on AbuseIPDB about this ip regarding SSH brute force attacks.

<img width="1575" height="510" alt="image" src="https://github.com/user-attachments/assets/f2c444e3-8c98-4a9a-bd50-76c0d31af151" />

<img width="490" height="286" alt="image" src="https://github.com/user-attachments/assets/f42aaacd-a743-4089-9417-a9ddac0a5d50" />


3. **Were Any Logins Successful?**:
    
    - Search for successful login attempts using keywords like `accepted`.
    - Ensure correct capitalization in queries.
    - No successful logins found found.
    <img width="501" height="410" alt="image" src="https://github.com/user-attachments/assets/c98e9ecd-bc13-4f57-8b21-e005ef1e8351" />


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
<img width="1100" height="546" alt="image" src="https://github.com/user-attachments/assets/85e69dc2-c1d6-415c-8694-928c1d33d9a4" />


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


<img width="698" height="268" alt="image" src="https://github.com/user-attachments/assets/111fd39d-139e-438e-bf66-a4e7e2d63d90" />

<img width="1098" height="747" alt="image" src="https://github.com/user-attachments/assets/b6c806de-fc6e-435d-a1cc-32b69deb8b0a" />




**Reference**
https://youtu.be/sXQ1hsAFX7U?si=Uw3wS9MFCwbo62J1

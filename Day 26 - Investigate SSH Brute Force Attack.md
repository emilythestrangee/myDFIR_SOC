### 1. Introduction

**Goal:** Learn how to investigate an SSH Brute Force alert, identify key indicators, and integrate the alert with osTicket to streamline incident tracking and response.

---

### 2. Configuring Kibana for Public Access

**Steps:**

- SSH into the ELK server and navigate to the Kibana configuration directory.
- Open `kibana.yml` and search for:
    `server.publicBaseUrl`
    
- Change the value to:
    `http://<publicIP-ELK>:5601`
    
- Save the file.
- Restart the Kibana service to apply the changes.



---

### 3. Accessing and Editing the SSH Brute Force Rule

**Steps:**

- Log in to the Elastic GUI.
- Navigate to **Security** → **Rules**.
- Click on **Detection Rules (SIEM)** and open the SSH Brute Force rule.
- Click **Edit rule settings** → **Actions** tab.
- Add **Webhook** as the action and select the osTicket connector.
- Change the **Action frequency** to _For each alert_ (for testing).
- In the body, use the osTicket XML template and insert dynamic variables such as `{{rule.name}}`, `{{rule.url}}`, and `{{alert.id}}`.


**Example Body:**

`<?xml version="1.0" encoding="UTF-8"?> <ticket alert="true" autorespond="true" source="API">     <name>Angry User</name>     <email>api@osticket.com</email>     <subject>{{rule.name}}</subject>     <message type="text/plain"><![CDATA[         Rule URL: {{rule.url}}         Alert ID: {{alert.id}}         Message content here     ]]></message> </ticket>`

---

### 4. Testing the Webhook Integration

**Steps:**

- If no alert is generated naturally, simulate an SSH brute force attack to trigger the rule.
- Verify that a ticket is automatically created in osTicket.
- Assign the ticket to an analyst for further investigation.

---

### 5. Investigative Methodology

**Key Questions to Guide the Investigation:**

- **Is the IP Known for Brute Force Activity?**
    
    - Use AbuseIPDB, GreyNoise Intelligence, and VirusTotal to check reputation.
    - Example: An IP reported over 20,000 times with SSH brute force tags is highly suspicious.
    
- **Were Any Other Users Affected by This IP?**
    
    - Search the IP in Kibana’s **Discover** tab.
    - Check for distinct user accounts targeted.
    - Example: `root`, `testuser`, and `ts3server`.
    
- **Were Any Logins Successful?**
    
    - Filter logs with:
        
        `<attacker_IP> AND system.auth.ssh.event : "Accepted"`
        
    - If no matches, no successful logins occurred.
    
- **What Activity Occurred After Successful Logins?**
    
    - If logins were successful, check for:
        
        - Downloaded payloads
        - Execution of discovery commands
        - Malicious binaries or persistence mechanisms

---

### 6. Closing the Alert

**Steps:**

- **Document Findings:**
    
    - Summarize details such as IP reputation, number of attempts, affected users, and results.
    - Example: “Malicious IP detected, multiple failed login attempts, no successful authentication.”

- **Integrate with Ticketing:**
    
    - Go to **Rules**.
    - Select the SSH Brute Force rule and verify Webhook configuration.
    - This ensures future alerts automatically generate tickets.

---

### 7. Reviewing and Closing Tickets

**Steps:**

- **Review Ticket Details:**
    
    - Confirm subject and message reflect the rule name and relevant information.
    
- **Assign and Document:**
    
    - Assign the ticket to yourself or another analyst.
    - Add investigation notes and findings.
    
- **Resolve and Close:**
    
    - Mark the ticket as _Resolved_ and post a closing reply.
    - Closed tickets can be reviewed under the **Closed** tab in osTicket for audit purposes.
        

---

### 8. Conclusion

- SSH brute force alerts should always be analyzed to assess potential compromise.
    
- Checking IP reputation provides fast context on attack severity.
    
- Integrating alerts with osTicket improves documentation, visibility, and escalation.
    
- Automating ticket creation increases efficiency in real-world SOC workflows.
**Reference**
https://youtu.be/sXQ1hsAFX7U?si=Uw3wS9MFCwbo62J1
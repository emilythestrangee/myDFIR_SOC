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

![[Pasted image 20251018234633.png]]

---

### 3. Investigative Methodology

**Key Questions:**

- **Is the Domain or Subdomain Suspicious?**
    
    - Check if the queried domain or subdomain is known for malicious activity.
    - Use reputation services such as AbuseIPDB, VirusTotal, or GreyNoise.
    - Example: A domain flagged for data exfiltration by multiple engines is a strong indicator.
    
- **Are There High Volumes of Unusual DNS Requests?**
    
    - Use Kibana Discover to filter by the destination domain.
    - Look for excessive TXT or NULL record queries over a short period.
    - Example: 2,000+ queries in 10 minutes may indicate exfiltration.

- **What Endpoint or Host Generated the Requests?**
    
    - Identify the source IP and hostname.
    - Check for patterns like service accounts or critical servers generating requests.

- **Is the Query Pattern Encoded or Structured?**
    
    - Look for Base64-like strings or long subdomains.
    - Example: `ZXhhbXBsZS5leGZpbC5jb20` can indicate encoded payloads.

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
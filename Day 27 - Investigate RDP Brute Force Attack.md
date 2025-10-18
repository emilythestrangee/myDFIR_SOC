
### 1. Introduction

**Objective:** Learn how to identify and investigate RDP brute force attempts, trace the source, and understand the scope of potential compromise.

---

### 2. Accessing and Reviewing Alerts

**Steps:**

- **Locate the Alert:**
    
    - Navigate to **Security → Alerts**.
    - Filter for alerts related to **RDP login failures** or brute force detection.
    - Select the alert you want to investigate.
![[Pasted image 20251019012906.png]]
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
![[Pasted image 20251019012614.png]]


- **Which Accounts Were Targeted?**
    
    - Use the SIEM to search for all login attempts from that IP.
    - Identify all users that were targeted to understand the scope.

![[Pasted image 20251019012725.png]]

![[Pasted image 20251019012953.png]]


- **Were Any Logins Successful?**
    
    - Look for successful RDP logins using event codes such as **4624** (Windows Logon Success).
    - Pay attention to the session type and logon IDs for tracking post-login activity.

![[Pasted image 20251019013020.png]]

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

**Steps to configure Elastic instance to push alerts to osTicket so we can automatically generate a ticket on osTicket for each alert generated (same as day 26):**

- **Configure Alert Routing:**
    
    - Under “Rules,” modify our existing rule for RDP brute force attacks and add an “Action”.

![[Pasted image 20251019013221.png]]
- **Customize Ticket Details:**
    
    - Include relevant variables such as domain, source IP, and rule name.
    - Remove unnecessary fields like attachments if not needed.

- **Test Integration:**
    
    - Save the rule configuration.
    - Run a test to ensure tickets are being created correctly.

### 7. Reconstructing the Kali-based Attack

Lastly, we will investigate the brute force attack originating from a Kali VM we used on day 21. 

**Approach:**

- The Kali session left traces (e.g., a Mythic agent install and related logs). Use those artifacts to bound the search.
    
- Use the timeline of the Mythic agent deletion (if present) as an _end_ marker for the suspicious activity. Treat it as the endpoint to limit noise from unrelated past logins from your personal IP.


**Practical steps:**

1. Search for successful logons (`event.code:4624`) with the target username and your personal IP, but restrict the end time to the agent deletion event. This narrows results to the likely Kali activity instead of all your normal logins.
    
2. From the returned logon events, start with the earliest one and copy the **TargetLogonId**.
    
3. Inspect the session tied to that logon ID. Short-lived successes that immediately log off may indicate automated tools or scans (e.g., Crowbar stopping when it finds a password).
    
4. If the first logon ID yields limited findings, iterate through the other returned logon IDs until you find one that shows richer activity (process creations, credential accesses).
    

**What to look for in the session:**

- Events showing credentials being read (possible credential-dump activity).
    
- PowerShell execution or script runs (common for post-exploit activity).
    
- Windows Defender being disabled or tampered with.
    
- Network connections established back to your Kali VM (in this lab, you expect an established connection on a non-standard port such as 9999).
    
- File creation events that correspond to the Mythic agent binary or related payloads.
    

**Narrowing to the precise timeframe:**

- Use the successful logon timestamp as the _start_ and the agent deletion timestamp as the _end_ to build a “between” time window.
    
- Limit queries to relevant event providers to reduce noise: for example `Microsoft-Windows-Sysmon`, `Microsoft-Windows-Security-Auditing`, and `Microsoft-Windows-Windows Defender`.
    

**Outcome observations (typical for the lab setup):**

- Multiple credential read events and discovery/enumeration commands following the successful login.
    
- Defender being disabled around the same timeframe.
    
- A network connection from the host to the Kali IP (port 9999) used to fetch or stage the Mythic agent.
    
- A file creation event indicating the agent binary or dropper was written to disk.

**Reference**
https://youtu.be/l9KA6dPdOs8?si=w4SZCr8RGoZv7R1K
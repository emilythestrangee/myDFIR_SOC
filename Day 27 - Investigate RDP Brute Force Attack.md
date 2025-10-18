
### 1. Introduction

**Objective:** Learn how to identify and investigate RDP brute force attempts, trace the source, and understand the scope of potential compromise.

---

### 2. Accessing and Reviewing Alerts

**Steps:**

- **Locate the Alert:**
    
    - Navigate to **Security → Alerts**.
    - Filter for alerts related to **RDP login failures** or brute force detection.
    - Select the alert you want to investigate.
<img width="512" height="401" alt="image" src="https://github.com/user-attachments/assets/2e7513e3-fd30-4db1-8dec-bb1bf0e033d9" />

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
<img width="882" height="685" alt="image" src="https://github.com/user-attachments/assets/39ec6303-b88a-490f-ab41-295d1e411f30" />



- **Which Accounts Were Targeted?**
    
    - Use the SIEM to search for all login attempts from that IP.
    - Identify all users that were targeted to understand the scope.

<img width="1655" height="311" alt="image" src="https://github.com/user-attachments/assets/6766e2f8-7cee-4959-b44f-618c80dbf167" />


<img width="198" height="231" alt="image" src="https://github.com/user-attachments/assets/88a63fe1-2b71-41f2-b582-67d275037204" />



- **Were Any Logins Successful?**
    
    - Look for successful RDP logins using event codes such as **4624** (Windows Logon Success).
    - Pay attention to the session type and logon IDs for tracking post-login activity.

<img width="1100" height="399" alt="image" src="https://github.com/user-attachments/assets/84c58fae-06b6-4912-bafe-92be9e5af34f" />


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

<img width="1030" height="1035" alt="image" src="https://github.com/user-attachments/assets/3e945107-6a65-4612-9ad5-b680f23211be" />

- **Customize Ticket Details:**
    
    - Include relevant variables such as domain, source IP, and rule name.
    - Remove unnecessary fields like attachments if not needed.

- **Test Integration:**
    
    - Save the rule configuration.
    - Run a test to ensure tickets are being created correctly.

### 7. Reconstructing the Kali-based Attack

Lastly, we will investigate the brute force attack originating from a Kali VM we used on day 21. 
The Kali session left traces (e.g., a Mythic agent install and related logs). Use those artifacts to bound the search.

**Investigation Flow:**

- Searched for successful Windows logons for the account I used with `event.code:4624` and the ip. This returned multiple sessions.
    
- I inspected the earliest session first and copied its `TargetLogonId`. That session showed a successful logon that immediately logged off — consistent with an automated scan/tool, since it all happened within a difference of a couple of seconds. (This matched the Crowbar behavior we ran when it found the password.)
    <img width="733" height="275" alt="image" src="https://github.com/user-attachments/assets/8202571d-12cd-4c1e-9595-6385254c8008" />

- I went back to the list of sessions to get some `TargetLogonId` values. The first few had nothing interesting, but the next `TargetLogonId` I tried produced many related events — lots of process creation and activity logged.
    
- Immediately after that successful logon I found events indicating **credentials were read** (credential-access indicators recorded).
<img width="905" height="187" alt="image" src="https://github.com/user-attachments/assets/3d0cd4f9-830e-4658-9104-2663b12ec9ab" />

    
- I also observed **PowerShell** process activity in that same session (command lines and script execution entries).

<img width="716" height="356" alt="image" src="https://github.com/user-attachments/assets/7e3397df-864a-4e8a-9309-34ae89b48cdc" />


- Found a lot of credentials reading and enumerations, if we remember we ran some discovery commands.

<img width="632" height="185" alt="image" src="https://github.com/user-attachments/assets/0bdc51ea-578d-4d1d-bccb-ed7728c19951" />

- I also observed Windows Defender being disabled during the window.

<img width="735" height="206" alt="image" src="https://github.com/user-attachments/assets/30022b5b-ad37-4122-aa87-c8e9f06a8047" />

- Searching network events in the same interval revealed an established connection back to the Kali host  on **port 9999** — the same port we used to pull down the Mythic agent.
<img width="411" height="349" alt="image" src="https://github.com/user-attachments/assets/c7e4193a-cdd5-4e63-95da-4800d08672f9" />

- Finally, I located a file-creation event that corresponded to the Mythic agent binary being written to disk.
    
<img width="655" height="194" alt="image" src="https://github.com/user-attachments/assets/c561e98d-b5bf-472f-957b-9af34f3ccefd" />


**What to look for in the session:**

- Events showing credentials being read (possible credential-dump activity).
- PowerShell execution or script runs (common for post-exploit activity).
- Windows Defender being disabled or tampered with.
- Network connections established back to your Kali VM (in this lab, you expect an established connection on a non-standard port such as 9999).
- File creation events that correspond to the Mythic agent binary or related payloads.


**Outcome observations:**

- Multiple credential read events and discovery/enumeration commands following the successful login.
    
- Defender being disabled around the same timeframe.
- A network connection from the host to the Kali IP (port 9999) used to fetch or stage the Mythic agent.
- A file creation event indicating the agent binary or dropper was written to disk.

**Reference**
https://youtu.be/l9KA6dPdOs8?si=w4SZCr8RGoZv7R1K

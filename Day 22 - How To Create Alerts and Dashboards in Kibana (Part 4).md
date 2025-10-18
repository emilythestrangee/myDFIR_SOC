
### 1. Introduction

Goal: Create an alert and dashboard in Kibana to detect Mythic activity.  This includes detecting process creation events for the Mythic payload and building visualizations for monitoring suspicious behavior.

---

### 2. Setting Up the Alert

**Steps:**

1. **Access Elastic Web GUI**
    
    - Click the hamburger icon.
    - Select **Discover**.
    - Create a new search to reset the view.
    
2. **Query for Mythic C2 Activity**
    
    - Search for the binary:
        `svchost-emaan.exe`
    
    - Set the time range to **30 days** to capture all relevant events.
    - Sort events from **old to new**.
    
    <img width="521" height="216" alt="image" src="https://github.com/user-attachments/assets/7d44d9dd-539a-48e0-9f25-6a02534db210" />


3. **Identify Relevant Events**
    
    - Look for events with `event.code: 11` (file creation).
    - Note the **target file name**, **username**, and **process GUID**.

<img width="786" height="325" alt="image" src="https://github.com/user-attachments/assets/4848558a-ccb8-4338-9473-f1d13172270d" />

3. **Focus on Process Create Events**
    
    - Change the query to look for `event.code: 1` (process create).
    - Copy the SHA-1 hash and run it through OSINT (e.g., VirusTotal).
    
4. **Create Alert Criteria**
    
    - Modified query:
        `event.code:1 and (winlog.event_data.OriginalFileName:"Apollo.exe" or winlog.event_data.Hashes:*FD6A88F718337FA6611C067693F65970D6EE073EC548A31D95C3D2F814296D2C*)`
        
    - Save the search as:
        Mythic-Apollo-Process-Create`


---

### 3. Building the Alert Rule

**Steps:**

1. **Create a New Rule**
    
    - Go to **Security → Detection Rules**.
    - Click **Create New Rule**.
    - Select **Custom Query** and paste the saved query.
    
2. **Add Required Fields**
    
    - `@timestamp`
    - `winlog.event_data.User`
    - `winlog.event_data.ParentImage`
    - `winlog.event_data.ParentCommandLine`
    - `winlog.event_data.Image`
    - `winlog.event_data.CommandLine`
    - `winlog.event_data.CurrentDirectory`
    
3. **Finalize and Enable Rule**
    
    - Name: `MyDFIR-Mythic-C2-Apollo-Agent-Detected`
    - Severity: **Critical**
    - Schedule: **Every 5 minutes**
    - Enable the rule.

<img width="601" height="436" alt="image" src="https://github.com/user-attachments/assets/c5af2b8a-a80e-4b66-bb8e-12ab49141f37" />


---

### 4. Creating the Dashboard

**Steps:**

1. **Define Dashboard Panels**
    
    - **Process Create Events:** Event ID `1` for PowerShell, CMD, and rundll32.
    - **External Network Connections:** Event ID `3`.
    - **Windows Defender Activity:** Event ID `5001`.
    
---

### 5. Queries for Dashboard Panels

#### a. Process Create Events

`event.code:1 and event.provider:"Microsoft-Windows-Sysmon" and (powershell or cmd or rundll32)`

**Fields:**

- `@timestamp`
- `winlog.event_data.User`
- `winlog.event_data.ParentImage`
- `winlog.event_data.ParentCommandLine`
- `winlog.event_data.Image`
- `winlog.event_data.CommandLine`
- `winlog.event_data.CurrentDirectory`

<img width="602" height="403" alt="image" src="https://github.com/user-attachments/assets/0de16abf-0bb8-451f-8c08-d04666a0c05d" />

---

#### b. External Network Connections

`event.code:3 and event.provider:"Microsoft-Windows-Sysmon" and winlog.event_data.Initiated:true`

**Fields:**

- `winlog.event_data.Image`
- `winlog.event_data.SourceIp`
- `winlog.event_data.DestinationIp`
- `winlog.event_data.DestinationPort`

<img width="1100" height="336" alt="image" src="https://github.com/user-attachments/assets/ea8be63a-db28-414d-838a-5cda20f89b5c" />

---

#### c. Windows Defender Activity

`event.code:5001 and event.provider:"Microsoft-Windows-Windows Defender"`

**Fields:**

- `host.hostname`
- `winlog.event_data.Product Name`
- `event.code`

<img width="476" height="345" alt="image" src="https://github.com/user-attachments/assets/d8f57aec-e24f-4904-b5ae-5c806febff6b" />

---

### 6. Building the Dashboard

**Steps:**

1. Go to **Analytics → Dashboards**.
2. Create a new dashboard.
3. Add visualizations for each query.
4. Include the relevant fields for each visualization.
5. Save each panel.

---

### 7. Finalizing the Dashboard

- **Panel Names:**
    
    - `Process Created (PowerShell, CMD, Rundll32)`
    - `Process Initiated Network Connections`
    - `Microsoft Defender Disabled`
    
- **Dashboard Name:**
    
    `MyDFIR-Suspicious-Activity`
    

**Reference**
https://youtu.be/WcVuUamMApA?si=Ccww-7i6zb-si3Uc

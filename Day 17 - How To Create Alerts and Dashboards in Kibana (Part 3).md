
## 1. Introduction

Goal: Build a **map and table visualization** in Kibana to track and display **RDP authentication activity** (both failed and successful attempts) from the Windows server ,similar to the SSH activity dashboard we created earlier.

---

## 2. Recall Saved Queries

Navigate to **Discover**→**Open** and open the previously saved query i.e, **RDP failed activity** (`event.code:4625`).

---

## 3. Create a Map for RDP Failed Authentications

**Steps**:

1. Navigate to **Maps** from the hamburger menu.
2. Paste the saved query for RDP failed activity in the search bar:
    
    `event.code:4625 AND agent.name:MYDFIR-WIN-emaan`
    
3. Adjust the time range to **Last 7 days** and click **Refresh**.
4. Click **Add Layer** → choose **Choropleth**.
5. In the settings:
    
    - Boundary: **World Countries**
    - Data view: `alert-security.alerts...`
    - Join field: `source.geo.country_iso_code`

6. Click **Add and continue** → **Save** the map.
7. Add it to the **existing dashboard**.
![[Pasted image 20251018005811.png]]

![[Pasted image 20251018005929.png]]

---

## 4. Create a Map for RDP Successful Authentications

**Steps**:

1. Go to **Discover** and modify the query to:
    
    `event.code:4624 AND (winlog.event_data.LogonType:10 OR winlog.event_data.LogonType:7)  AND agent.name:MYDFIR-WIN-emaan`
    
    - `4624` → Successful authentication
    - LogonType `10` (RemoteInteractive) and `7` (Unlock)
        
2. Save this query as **“Challenge-RDP Successful Authentication Activity”**.
3. Duplicate the **previous RDP failed map** on the dashboard.
4. Update the map’s query and title accordingly.

---

## 5. Add Table Visualizations for Detailed Logs



### 5.1 SSH Failed Activity Table

**Steps**:

1. Click **Create Visualization** → select **Table**.
2. Query:
    
    `system.auth.ssh.event:* AND agent.name:"Challenge-Linux-Steve"  AND system.auth.ssh.event:"Failed"`
    
3. Add fields:
    
    - `@timestamp`
        
    - `user.name`
        
    - `source.ip`
        
    - `source.geo.country_name`
        
4. Configure rows:
    
    - Top 10 values per field
        
    - Uncheck **Group remaining values as “Other”**
        
5. Sort by **Count of records** in descending order.
6. Save as **“Challenge-SSH Failed Authentication Activity (Table)”**.

---

### 5.2 SSH Successful Activity Table

- Duplicate the failed SSH table.
- Update the query to:
    
    `system.auth.ssh.event:* AND agent.name:"Challenge-Linux-Steve"  AND system.auth.ssh.event:"Accepted"`
    
- Change the title to: **“Challenge-SSH Successful Authentication Activity (Table)”**.

---

### 5.3 RDP Failed Activity Table

- Duplicate the SSH failed table.
- Update the query to:
    
    `event.code:4625 AND agent.name:"Challenge-WIN-Haji"`
    
- Change the title to: **“Challenge-RDP Failed Authentication Activity (Table)”**.


---

### 5.4 RDP Successful Activity Table

- Duplicate the RDP failed table.
    
- Update the query to:
    
    `event.code:4624 AND (winlog.event_data.LogonType:10 OR winlog.event_data.LogonType:7)  AND agent.name:"Challenge-WIN-Haji"`
    
- Change the title to: **“Challenge-RDP Successful Authentication Activity (Table)”**.
    

---

## 6. Finalizing the Dashboard

- You should now have:
    
    - 🗺 2 Choropleth maps (RDP Failed & Successful)
        
    - 🗺 2 SSH maps from previous steps
        
    - 📊 4 tables (SSH Failed/Success, RDP Failed/Success)
        
- Review all titles and queries to ensure they’re correct.
    
- Click **Save** in the top right corner of the dashboard.
    

✅ **End Result:** A centralized, interactive dashboard showing **SSH and RDP activity**, both failed and successful, with geographical and table details (username, IP, country).


**Reference**
https://youtu.be/pAfIi6Z6a2g?si=rrv48fwEZepmsiQh
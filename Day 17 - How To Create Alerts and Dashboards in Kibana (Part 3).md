
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
        
1. Save this query as **“RDP Successful Activity”**.
2. Duplicate the **previous RDP failed map** on the dashboard.
3. Update the map’s title to **RDP Successful Authentications**.
4. Optionally, update the **RDP Failed Authentication** map title to **RDP Failed Authentications.

![[Pasted image 20251018010937.png]]



---

## 5. Add Table Visualizations for Detailed Logs
### 5.1 SSH Failed Activity Table

**Steps**:

1. Go to **Create Visualization** → select **Table** from the dropdown menu.
2. Query:
    
    `system.auth.ssh.event:* AND agent.name:MYDFIR-Linux-emaan AND system.auth.ssh.event:Failed`
    
3. Add fields by dragging and dropping them respectively:
    
    - `user.name`
    - `source.ip`
    - `source.geo.country_name`
    
4. Configure rows:
    
    - Top 10 values per field by changing "Number of values"
    - Uncheck **Group remaining values as “Other”**
    
5. Sort by **Count of records** in descending order.
6. Save as **“SSH Failed Authentications [Table]”**.


### 5.2 SSH Successful Activity Table

- Duplicate the failed SSH table.
- Update the query to:
    
    `system.auth.ssh.event:* AND agent.name:MYDFIR-Linux-emaan  AND system.auth.ssh.event:Accepted`
    
- Change the title to: **“SSH Successful Authentications [Table]”**.

![[Pasted image 20251018012645.png]]

---

### 5.3 RDP Failed Activity Table

- Duplicate the SSH failed table.
- Update the query to:
    
    `event.code:4625 AND agent.name:MYDFIR-WIN-emaan`
    
- Change the title to: **“RDP Failed Authentications [Table]”**.


### 5.4 RDP Successful Activity Table

- Duplicate the RDP failed table.
- Update the query to:
    
    `event.code:4624 AND (winlog.event_data.LogonType:10 OR winlog.event_data.LogonType:7)  AND agent.name:MYDFIR-WIN-emaan
    
- Change the title to: **“RDP Successful Authentications [Table]”**.

![[Pasted image 20251018013134.png]]

---

## 6. Finalizing the Dashboard

- You should now have:
    
    - 🗺 2 RDP maps (Failed & Successful)
    - 🗺 2 SSH maps from previous days
    - 📊 4 tables (SSH Failed/Success, RDP Failed/Success)
        
- Review all titles and queries to ensure they’re correct.
- Click **Save** in the top right corner of the dashboard.


**End Result:** A centralized, interactive dashboard showing **SSH and RDP activity**, both failed and successful, with geographical and table details (username, IP, country).


**Reference**
https://youtu.be/pAfIi6Z6a2g?si=rrv48fwEZepmsiQh

## Introduction

**Goal:** Create alerts for SSH brute force attempts and visualize attack sources using Kibana dashboards. This allows tracking failed and successful login attempts and identifying the geographic origin of attacks.

---

## Query Logs for Brute Force Activity

**Steps:**

- Log into **Elasticsearch Web GUI** → **Discover**.
- Filter by your Linux server by clicking **agent.name** (e.g., `MYDFIR-Linux-emaan`).
- Set the time range for initial analysis (Today).

---

## Identify Key Fields

**Fields to Track:**
- **Failed Attempts:** Detect authentication failures.
- **User:** Identify usernames targeted.
- **Source IP:** Track origin of login attempts.
- **Location:** Geo-location of source IPs.

---

## Filter for Failed Attempts

**Steps:**

- Look for fields that relate to the authentication status of each log record.
- Use `system.auth.ssh.event` to filter failed, invalid, or accepted login attempts.
- Add `system.auth.ssh.event` as a column by clicking on the plus button next to it.
- Apply the filter `system.auth.ssh.event:*` to narrow results.
- Highlight failed attempts and add them to the filter for analysis.

![[Pasted image 20251017225406.png]]

![[Pasted image 20251017230430.png]]

---

## Add Relevant Columns

**Steps:**

- Add `user.name` to track usernames.
    
- Add `source.ip` to track source IP addresses.
    
- Optionally add `source.geo.country_name` for the country of origin.
    
- The table should display: User, IP, Country, and authentication status.
    

---

## Save the Query

- Save the search as **"SSH Failed Activity"** for reuse in alerts and dashboards.
    

---

## Create Alert

**Steps:**

- Click **Alerts** → **Create Search Threshold Rule**.
    
- Name the alert (e.g., `Challenge - SSH Brute Force Activity - Steve`).
    
- The saved query is automatically applied.
    
- Set threshold: trigger if more than **5 failed attempts** occur within **5 minutes**.
    
- Test the query to ensure it matches documents.
    
- Save the rule.
    

---

## Create Dashboard

**Steps:**

- Navigate to **Maps** under the **Analytics** menu.
    
- Filter data using the saved query:
    

`system.auth.ssh.event:* AND agent.name:"Challenge-Linux-Steve" AND system.auth.ssh.event:"Failed"`

- Click **Add Layer** → select **Choropleth Layer**.
    
- Configure layer:
    
    - **Boundaries Source:** Administrative Boundaries → World Countries
        
    - **Statistics Source:** Data view corresponding to your index
        
    - **Join Field:** `source.geo.country_iso_code`
        
- Save the map as **"Challenge-SSH Failed Authentication Activity"**.
    
- Add it to a new dashboard named **"Challenge SSH Authentication Activity"**.
    

---

## Create Successful Authentication Dashboard

**Steps:**

- Duplicate the failed attempts map.
    
- Rename it to **"Challenge-SSH Successful Authentication Activity"**.
    
- Update the query to filter successful logins:
    

`system.auth.ssh.event:* AND agent.name:"Challenge-Linux-Steve" AND system.auth.ssh.event:"Accepted"`

- Save and add it to the same dashboard.
    
- Adjust the time filter to visualize recent activity (e.g., Last 12 hours).
    

---

## Outcome

- The dashboard now shows failed and successful SSH login attempts by source country.
    
- Alerts notify when brute force thresholds are exceeded.
    
- This setup allows proactive monitoring and geographic tracking of login attempts.


**Reference**
https://youtu.be/AdUMhT1l1eY?si=6FJSv4Hiohr-5d9R
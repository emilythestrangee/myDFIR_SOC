
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

<img width="582" height="320" alt="image" src="https://github.com/user-attachments/assets/45d7dc23-9018-4de2-9d4c-e4bf9dd520b1" />


<img width="1148" height="400" alt="image" src="https://github.com/user-attachments/assets/a9e950db-5754-4cd7-ba16-f814ee208d34" />


---

## Add Relevant Columns

**Steps:**

- Add `user.name` to track usernames.
- Add `source.ip` to track source IP addresses.
- Optionally add `source.geo.country_name` for the country of origin.
- The table should display: User, IP, Country, and authentication status.

<img width="539" height="446" alt="image" src="https://github.com/user-attachments/assets/71d49ba6-a9d8-4c6a-8fc8-9f2b09bce99e" />


<img width="285" height="219" alt="image" src="https://github.com/user-attachments/assets/47fa1771-4b69-45d3-b32e-c6bf05678e13" />


<img width="294" height="172" alt="image" src="https://github.com/user-attachments/assets/3852f1f7-bbce-487c-9e9e-91d923865218" />


<img width="1169" height="348" alt="image" src="https://github.com/user-attachments/assets/cab35aa5-af30-40be-a9d7-fb13b3264f14" />


---
## Save the Query

- Save the search as **"SSH Failed Activity"** for reuse in alerts and dashboards.
<img width="600" height="566" alt="image" src="https://github.com/user-attachments/assets/252b0698-d689-43cf-a1f1-5b9f13b56fa6" />


---

## Create Alert

**Steps:**

- Click **Alerts** → **Create Search Threshold Rule**.
- Name the alert (e.g., `MyDFIR-SSH Brute Force Activity-emaan`).
- The saved query is automatically applied.
- Set threshold: trigger if more than **5 failed attempts** occur within **5 days**.
- Test the query to ensure it matches documents.
- Save the rule.

<img width="581" height="485" alt="image" src="https://github.com/user-attachments/assets/f079667f-56c8-4175-a49a-a8a1858ab2bf" />


<img width="572" height="103" alt="image" src="https://github.com/user-attachments/assets/a46ec818-f428-4ab4-81bd-2e96ac2e0e24" />


---

## Create Dashboard

**Steps:**

- Navigate to **Maps** under the **Analytics** menu.
- Filter data using the saved query and paste it into the search bar:

`system.auth.ssh.event:* and agent.name:MYDFIR-Linux-emaan and system.auth.ssh.event:Failed`

- Click **Add Layer** → select **Choropleth Layer**.
- Configure layer:
    
    - **Boundaries Source:** Administrative Boundaries → World Countries
    - **Statistics Source:** Data view corresponding to your index
    - **Join Field:** `source.geo.country_iso_code`

<img width="506" height="717" alt="image" src="https://github.com/user-attachments/assets/0c326d9d-ce61-45fd-b362-7412e4138615" />

- Save the map as **"SSH Failed Authentications - Network Map"**.
- Add it to a new dashboard named **"MyDFIR-Authentication-Activity"**.

  
<img width="665" height="341" alt="image" src="https://github.com/user-attachments/assets/370e4682-d4c7-4635-999d-5c246c5f2671" />

<img width="1100" height="484" alt="image" src="https://github.com/user-attachments/assets/b47b56fc-052f-4234-810a-1918d0bb6472" />


---

## Create Successful Authentication Dashboard

**Steps:**

- Duplicate the failed attempts map.
- Rename it to **"SSH Successful Authentications"**.
- Update the query to filter successful logins:
    

`system.auth.ssh.event:* and agent.name:MYDFIR-Linux-emaan and system.auth.ssh.event:Accepted

- Save and add it to the same dashboard.
- Adjust the time filter to visualize recent activity (eg: Last 30 days).

<img width="1100" height="315" alt="image" src="https://github.com/user-attachments/assets/f4015c79-04b5-4f32-b4c3-7261ea6cf609" />


---
## Outcome

- The dashboard now shows failed and successful SSH login attempts by source country.
- Alerts notify when brute force thresholds are exceeded.
- This setup allows proactive monitoring and geographic tracking of login attempts.


**Reference**
https://youtu.be/AdUMhT1l1eY?si=6FJSv4Hiohr-5d9R

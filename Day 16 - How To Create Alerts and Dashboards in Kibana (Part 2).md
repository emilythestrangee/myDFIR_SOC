
## Introduction

**Goal:** Create targeted alerts and detection rules for brute force activity on the Windows server, and extend the previous SSH detection into more structured SIEM detection rules. This day covers both simple search-threshold alerts (from saved searches) and richer Threshold detection rules under Security → Rules.

---

## Query Windows Failed Logons (RDP)

Windows failed logons are represented by Event ID **4625**. Start in Discover to build and verify the query before converting it to an alert or a detection rule.

**Steps:**

- Open **Discover**.
- Filter by your Windows agent: click **agent.name** and select the Windows host (`MYDFIR-WIN-emaan`).
- Use the Event ID filter in the search bar:
    `event.code: 4625`
    
- Add useful columns to the table:
    - `source.ip`
    - `user.name` 
    
- Save the search (**"RDP Failed Activity"**).
    ![[Pasted image 20251017235704.png]]
![[Pasted image 20251017235722.png]]
---

## Create a Simple Search-Threshold Alert from Saved Search

**Steps:**

- With the saved search open, click **Alerts** (top-right) → **Create search threshold rule**.
- Name the alert (e.g., `MyDFIR-RDP Brute Force Activity-emaan`).
- Confirm the saved query is pre-populated in the rule.
- Configure threshold parameters:
    
    - Trigger when **IS ABOVE 5** (documents)
    - For the **last 5 minutes** (or your preferred window)
    - Schedule checks to run every **1 minute** (common choice for authentication alerts)
    
- Test the query using the built-in Test feature to validate it matches expected documents.
- Save the alert.
![[Pasted image 20251018000449.png]]

---

## Create Detailed Detection Rules (SIEM) — RDP (Windows) Threshold Detection Rule


Alerts created under Stack Management can notify you but may lack rich contextual fields. Detection rules under **Security → Rules → Detection rules** allow grouping, required fields, severity, and more informative alerts.

**Steps to create a Threshold detection rule (RDP first, then SSH):**

1. Go to **Security → Rules → Detection rules** (SIEM).
2. Click **Create new rule**.
3. Select **Threshold** as the rule type.
4. Enter the query you verified in Discover. 
    
    `event.code:4625 AND agent.name: "MYDFIR-WIN-emaan" AND user.name: "Administrator"`
    ![[Pasted image 20251018001559.png]]
5. Configure **Threshold**: set threshold value to **5** (or your desired sensitivity).
6. Configure **Group by** fields so the rule aggregates per actor/source. Common choices:
    
    - `source.ip`
    - `user.name`  
        Grouping provides per-IP and per-user counts rather than global counts.
        
7. Add **Required fields** to ensure alert output includes the most useful fields (e.g., `source.ip`, `user.name`, `host.hostname`).
8. Set rule metadata: name, description, severity (e.g., **medium**).
9. Set schedule/look-back: run every **1 minute**, look back **5 minutes** (or run every 1 minute with a 5-minute look-back).
10. Leave actions as default (or add notification actions).
11. Click **Create & enable rule**.

![[1_XcQzDkqoU0VhMIgWteQjSA.jpg]]

---

## Create SSH (Windows) Threshold Detection Rule

**Recommended query template:**

`system.auth.ssh.event:* AND agent.name: MYDFIR-Linux-emaan and system.auth.ssh.event: Failed and user.name: root`

**Steps:**

- Repeat the Threshold rule creation flow above using the RDP query.
- Group by `source.ip` and `user.name` so you get per-user, per-IP counts.
- Add required fields: `source.ip`, `user.name`, `host.hostname`, `winlog.event_data.LogonType` (if available).
- Set threshold to **5**, severity **medium**, schedule to run every **1 minute** with a 5-minute look-back.
- Create & enable the rule.
![[Pasted image 20251018003056.png]]

---

## Testing and Validation

**Steps to validate rules:**

- For RDP: attempt several failed RDP logins using a random password (from a test client) to reach the threshold and trigger the alerts.
- For SSH: attempt failed SSH auths (or use a log replay) to trigger the SSH threshold rule.
- After triggers, go to **Security → Alerts** (or Stack Management → Alerts) to inspect alert details. Confirm the alert contains the grouped fields (`source.ip`, `user.name`) and enough context for investigation.
- Tune thresholds and schedule/look-back windows based on observed noise and legitimate activity patterns.

![[Pasted image 20251018004048.png]]

---

## Investigation Guidance After an Alert

**Immediate steps:**

- Expand the alert to view required fields and example documents.
- Note offending `source.ip`, `user.name`, timestamps, and the host affected.
- Pivot to **Discover** or the host timeline and search for additional context (related events, process, parent process) around the alert time.
- If confirmed malicious, follow containment/remediation playbook (block IP, reset credentials, isolate host, gather forensic artifacts).

---

## Summary / Outcomes

- You created both simple search-threshold alerts and richer Threshold detection rules in the SIEM rules engine.
- Windows-focused detection uses `event.code:4625` for failed logons and `event.code:4624` for successful logons.
- Grouping by `source.ip` and `user.name` is essential to produce actionable alerts with per-attacker granularity.
- Schedule and threshold tuning are necessary to balance detection sensitivity vs. noise.
- Test alerts with controlled failed attempts and validate the alert payload before relying on them operationally.


**Reference**
https://youtu.be/11eBIfDeZ7k?si=qn7Ttut2WN41Ny_x
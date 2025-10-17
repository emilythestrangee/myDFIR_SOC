## Introduction

**Goal:** Install Elastic Agent on an Ubuntu SSH server to collect and forward logs to Elasticsearch for analysis. This step is similar to the Windows server installation but adapted for Linux systems.

---

## Create Agent Policy

**Steps:**

- Log into the **Elasticsearch Web GUI**.
    
- Click the hamburger menu → **Management** → **Fleet**.
    
- Navigate to **Agent Policies** → **Create Agent Policy**.
    
- Enter a name for the policy (`MyDFIR-Linux-Policy`).
    
- Click **Create Agent Policy**.
    
---

## Configure Policy

**Steps:**

- Select the system integration (`system-3`).
    
- Verify log collection paths:
    
    - Ubuntu/Debian: `/var/log/auth*`
    - Red Hat/CentOS: `/var/log/secure*`
        
- Ensure the policy supports all major Linux distributions (Debian, Ubuntu, Red Hat, CentOS). As we are using Ubuntu, we don’t have the “secure” path in our system. Navigating to the /var/log directory in our Ubuntu VM MYDFIR-Linux-emaan, we can see that only 'auth.log' is present not 'secure'.

![[Pasted image 20251017181422.png]]

![[Pasted image 20251017181850.png]]

---

## Enroll Agent

**Steps:**

- Navigate back to **Fleet → View All Agent Policies → Agents**.
    
- Click **Add Agent**.
    
- Select the newly created policy (e.g., `Challenge-Linux-Policy`).
    
- Choose **Enroll in Fleet → Linux Tar**.
    
- Configure firewall rules to allow communication between the Linux host and the Fleet/ELK server on required ports (e.g., 9200).
    
- Copy the installation command from the Fleet GUI and run it via SSH on the Linux server.
    
- Add the `--insecure` flag if using self-signed certificates.
    
- When prompted, send **Y** to run Elastic Agent as a service.
    

**Common Issue:**

- If the installation fails, check firewall rules to ensure the Linux server can communicate with the Fleet server.
    

---

## Verify Installation

**Steps:**

- Confirm the agent status in **Fleet**; it should show as **Healthy** with memory and other metrics.
    
- Navigate to **Discover** in Kibana to check incoming logs.
    
- Adjust the time filter to ensure recent logs are visible.
    
- Filter logs by `agent.name` to view data from your Linux agent.
    

---

## Analyze Logs

**Steps:**

- Identify errors and events using Kibana or SSH commands.
    
- Example: Find failed SSH login attempts on the Ubuntu server:
    

`grep -i failed /var/log/auth.log | grep -i root | cut -d ' ' -f 11`

- Add important fields to Kibana tables for structured analysis.


**Reference**
https://youtu.be/QHJr2-Kav4k?si=Lol4L_dwzyRZYfF5
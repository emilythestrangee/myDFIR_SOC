### 1. Introduction

**Goal:** Ensure all components of the home lab are fully functional, troubleshoot any issues, and consolidate all configurations.

- The final day focused on verifying connectivity, agent enrollment, and overall system readiness.
    
- Key tasks included troubleshooting Fleet Server, Elastic Agent enrollment, PHP MyAdmin configuration, and database setup for osTicket.
    

---

### 2. Troubleshooting and System Validation

- Verified firewall rules on Vultr instances and the Ubuntu VM, ensuring required ports were open (Kibana, Fleet, Elasticsearch).
    
- Checked the status of system services like Kibana and Elasticsearch to confirm proper operation.
    
- Adjusted Fleet Server settings and ensured Elastic Agent enrollment succeeded without errors.
    
- Resolved certificate issues using the `--insecure` flag when necessary.
    
- Validated connectivity by pinging the Fleet Server and confirming endpoints reported successfully.
    

---

### 3. Database and Ticketing Setup

- Configured PHP MyAdmin to allow access using the public IP and updated credentials.
    
- Created a database for osTicket and assigned privileges to the root account.
    
- Completed osTicket installation and ensured integration with Elastic alerts via webhooks.
    
- Tested ticket creation from alert triggers and verified audit trails in the ticketing system.
    

---

### 4. Deployment Summary

#### ELK Instance

- Ubuntu server deployed on Vultr hosting Elasticsearch and Kibana.
    
- Elasticsearch used for storing and querying logs; Kibana for dashboards and visualization.
    

#### Target Machines

- Windows Server 2022 and Ubuntu Server 24.02 deployed as endpoints for telemetry collection.
    
- Sysmon installed on Windows for detailed logging of process and network activity.
    
- Both servers exposed via SSH/RDP to collect brute force telemetry and conduct attack simulations.
    

#### Fleet and Elastic Agent

- Fleet server installed for centralized agent management.
    
- Elastic Agents deployed on Windows and Ubuntu endpoints to forward telemetry to Elasticsearch.
    

#### Command and Control Setup

- Mythic C2 server hosted on Ubuntu with Apollo agent deployed on Windows endpoint.
    
- Attack simulation conducted via Kali Linux and Mythic C2, demonstrating remote command execution and data exfiltration.
    

#### Alerts and Dashboards

- SSH and RDP brute force alerts created along with dashboards for visualization.
    
- Telemetry from Mythic C2 agent also monitored via dedicated alerts.
    
- Dashboards provided high-level views of authentication attempts, endpoint activity, and suspicious behavior.
    

#### Ticketing System Integration

- osTicket configured to automatically generate tickets for triggered alerts.
    
- Tickets allowed tracking, assigning, and documenting incident response actions.
    
- Integration satisfied Audit and Accountability requirements in the AAA security model.
    

#### Investigations

- Applied detection and investigation methodologies for:
    
    - SSH Brute Force Attacks
        
    - RDP Brute Force Attacks
        
    - Mythic C2 Activity
        
- Correlated events, process IDs, logon IDs, and network connections to reconstruct attack timelines.
    

#### Elastic Defend (EDR)

- Installed and configured Elastic Defend to enhance endpoint protection.
    
- Observed telemetry such as process trees, file creation events, and malware alerts.
    
- Configured response actions like host isolation and verified automated EDR responses.
    

---

### 5. Key Takeaways

- Hands-on experience with full SOC lab setup, including SIEM, EDR, ticketing system, and C2 environment.
    
- Learned the importance of documenting investigations, alerts, and configurations for analysis and review.
    
- Gained practical skills in alert creation, dashboard visualization, threat hunting, and incident response.
    
- Established a home SOC lab that can be reused for future learning, simulations, and skill refinement.
    

---

### 6. Conclusion

- The 30-day challenge provided both theoretical knowledge and practical skills in cybersecurity operations.
    
- The lab environment now enables continuous learning in detection, investigation, alerting, and endpoint security.
    
- This challenge serves as an excellent foundation for anyone looking to jump-start a career in SOC, DFIR, or cybersecurity operations.

**Reference**
https://youtu.be/o-eR-tJlbqE?si=peve6ZeF9vCeeq8k
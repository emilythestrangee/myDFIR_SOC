
### 1. Introduction

**Goal:** Install Elastic Defend EDR on a Windows host and observe the telemetry it generates. Focus on endpoint detection, malware alerts, and response capabilities.

- Elastic Defend is an Endpoint Detection and Response system that continuously monitors endpoints for suspicious behavior.

- It detects unusual files, unauthorized actions, and other indicators of compromise, alerting security teams in real-time.

- EDR allows analysts to visualize process trees, file activity, network connections, and respond to threats such as isolating a host.

---

### 2. Elastic Defend Integration

- The integration was added to the existing Windows agent policy via Fleet.
- Configuration included selecting Complete EDR to ensure full telemetry coverage.
- After deployment, the endpoint appeared under Security → Manage → Endpoints with its EDR policy active.
- A Windows machine restart was required to ensure Elastic Agent and ElasticEndpoint services were running correctly.

---

### 3. Observing Endpoint Telemetry

- The Mythic Agent executable previously deployed was disabled after the restart, as no persistence mechanisms were in place.
    
- Attempting to run the executable triggered an immediate malware alert and deletion by Elastic Defend.
    
- Searching the Discover tab for recent malware events showed logs with key details: file path, file hash, owner, and quarantine location.
    
- Alerts were visible under Security → Alerts, highlighting the malware prevention activity.
    

---

### 4. Process Tree and Alert Analysis

- The process tree visualization allowed inspection of parent-child relationships between processes.
    
- Elastic Defend successfully linked suspicious activity, like attempted execution of the Mythic Agent, back to the originating process.
    
- Relevant alert fields included process name, user, timestamp, and file hash — all essential for correlating malicious activity.
    

---

### 5. Response Actions

- A response action (host isolation) was configured in the alert rule.
    
- Testing confirmed that when attempting to download or execute the Mythic Agent again, Elastic Defend deleted the file instantly and triggered the isolation response.
    
- The Windows endpoint displayed the “Isolated” tag alongside the healthy status, confirming the action was executed.
    

---

### 6. Key Observations

- Elastic Defend can detect and prevent malicious files even on a free subscription, though some advanced features (like host isolation) require a trial or paid license.
    
- Alerts provide comprehensive telemetry including process, file, and network activity.
    
- The EDR effectively blocked the Mythic Agent, demonstrating real-time detection and prevention capabilities.
    
- Configuring response actions, such as isolation, enhances the security posture by immediately mitigating potential threats.
    

---

### 7. Conclusion

- Installing Elastic Defend provided visibility into endpoint activity and highlighted how an EDR reacts to malware attempts.
    
- The EDR blocked execution, generated alerts, and allowed for process and file correlation.
    
- This exercise showed the importance of endpoint protection in a SOC environment and reinforced real-time incident response practices.
    
- With Elastic Defend active, analysts can detect, investigate, and respond to threats efficiently, ensuring endpoints remain secure.
**Reference**
https://youtu.be/Ec-Ab8TbJKs?si=3q8KM65SbuYfHdVp
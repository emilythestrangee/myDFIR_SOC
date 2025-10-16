## Introduction

- Goal: Understand **Sysmon** and its role in endpoint visibility.
    
- Endpoint visibility is crucial for investigating potential compromises and identifying malicious activity.
    
- Default Windows logging exists but is often insufficient; additional monitoring tools like Sysmon help fill the gaps.
    

---

## What is Sysmon?

- **Sysmon** is a free Microsoft tool, part of the Sysinternals suite.
    
- Provides detailed telemetry for security-relevant events.
    
- Monitors activities such as:
    
    - Process creation and termination
        
    - Network connections
        
    - File creation and modification
        
    - Registry changes
        
- Customizable via a configuration file to control which events are logged.
    
- Latest version: 15.15 with support for 30 event types.
    
- Logs are stored in the Windows Event Log and can be collected/analyzed using ELK (Elasticsearch, Logstash, Kibana).
    

---

## Key Capabilities

- **Process Creation Logging**:
    
    - Tracks new processes and command-line arguments.
        
    - Logs parent and child process relationships.
        
    - Includes process hashes for verification and correlation.
        
- **Network Connection Monitoring**:
    
    - Logs outbound connections by processes.
        
    - Captures source/destination IPs, ports, and process responsible.
        
    - Disabled by default and must be enabled in the configuration.
        
- **File and Registry Monitoring**:
    
    - Tracks changes to file creation times, modifications, and deletions.
        
    - Monitors registry changes, including key/value creation, deletion, or modification.
        
- **Process Access and Thread Injection**:
    
    - Detects when a process accesses another process (Event ID 10).
        
    - Monitors creation of remote threads (Event ID 8), often used by malware for code injection.
        

---

## Important Event IDs to Know

- **Event ID 1 – Process Creation**: Logs new processes and their command-line arguments.
    
- **Event ID 3 – Network Connection**: Disabled by default; tracks process-related connections.
    
- **Event ID 6 – Driver Loaded**: Monitors drivers loaded into the system.
    
- **Event ID 7 – Image Loaded**: Tracks modules loaded by processes; disabled by default.
    
- **Event ID 8 – CreateRemoteThread**: Detects threads created in other processes (malware technique).
    
- **Event ID 10 – Process Access**: Tracks processes accessing others, commonly used to detect credential access attempts (e.g., LSASS).
    
- **Event ID 22 – DNS Queries**: Logs domain queries made by processes, useful for identifying compromised endpoints.
    

---

## Key Takeaways

- Sysmon provides **enhanced endpoint visibility** beyond default Windows logging.
    
- Central for SOC investigations and threat detection.
    
- Event logs from Sysmon can be ingested into ELK for further analysis and correlation.
    
- Enables detection of advanced techniques like process injection, unauthorized access, and suspicious network activity.


**Reference**
https://youtu.be/hpUnKjEFCoU?si=YaubGn6ZE-jaZTVd
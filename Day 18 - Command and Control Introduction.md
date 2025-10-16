
## Introduction

Goal: Learn what Command and Control (C2) is, why attackers use it, common frameworks, and practical next steps for setting up and detecting C2 activity in a lab environment.

---

## What is Command and Control (C2)

- C2 refers to the methods and channels adversaries use to communicate with compromised systems inside a victim network.
    
- Establishing C2 gives attackers the ability to run follow-on actions such as credential theft, lateral movement, data exfiltration, and deploying ransomware.
    
- MITRE documents many techniques for establishing C2—these techniques enable persistence and remote control of compromised hosts.
    

---

## Why C2 matters

- Without a reliable C2 channel, an attacker’s access is limited; with it they can escalate, pivot, and maintain presence.
    
- From a defender perspective, detecting and disrupting C2 is high priority because it breaks the attacker’s ability to orchestrate further operations.
    
- C2 activity often produces observable telemetry (network callbacks, unusual protocols, beaconing patterns) that can be monitored and alerted on.
    

---

## Common tools and frameworks

- **Metasploit (Rapid7)**
    
    - A widely used exploitation and post-exploitation framework. Often used for testing and initial footholds.
        
- **Cobalt Strike (Fortra)**
    
    - Commercial adversary-emulation platform commonly seen in real-world compromises. Known for robust post-exploitation features and team-server architecture.
        
- **Sliver (Bishop Fox)**
    
    - Open-source C2 alternative that supports multiple transport options (mTLS, HTTP(S), DNS, WireGuard). Designed as a modern, community-driven replacement for commercial tools.
        
- **Mythic**
    
    - Open-source C2 used in this lab. Built with modern tooling (Go, Docker) and a browser UI. Tracks payloads and supports many agent profiles, making it useful for red-team simulations in the challenge.
        

---

## Detection signals to watch for

- Repetitive outbound connections (beaconing) to uncommon hosts or suspicious domains.
    
- Unusual protocols or encrypted traffic to atypical endpoints.
    
- New or unexpected services spawning network connections (process → network correlation).
    
- DNS anomalies: frequent NXDOMAINs, unusual query patterns, or tunneled DNS.
    
- Alerts for known C2 tool signatures or YARA/snort/IDS rules that match C2 activities.
    

---

## Lab tasks / next steps (from the course)

- Build an attack scenario against a Windows server to simulate initial access and follow-on actions.
    
- Deploy a Mythic server in the lab environment (Docker-compose recommended).
    
- Generate and deploy an agent to a target host and confirm successful C2 callbacks and interactive control.
    

---

## Key takeaways

- C2 is the backbone of an attacker’s post-compromise activity; disrupting it severely limits an adversary.
    
- Familiarity with both commercial and open-source C2 frameworks helps in recognizing their behaviors and artifacts.
    
- In a SOC role, focus on network telemetry, process-to-network correlation, and DNS monitoring to detect C2.
    
- Hands-on exercises (deploying Mythic and observing agent behavior) are valuable for understanding both offensive operations and defensive detection.

**Reference**
https://youtu.be/WnOkhGNPmyA?si=Xtnv_GPzIaj_qVyQ
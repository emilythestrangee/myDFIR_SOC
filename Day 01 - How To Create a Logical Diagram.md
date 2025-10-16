
## Objective

The goal for Day 1 is to create the logical diagram for the myDFIR SOC Analyst challenge. This diagram helps visualize how data will flow, how the environment will be structured, and how components will interact with each other. The diagram serves as the foundation for the rest of the 30-day project.

---

## Components

- Elastic and Kibana Server
    
- osTicket Server
    
- Windows Server (RDP enabled)
    
- Ubuntu Server (SSH enabled)
    
- Fleet Server
    
- C2 Server (Mythic)
    
- Analyst Laptop
    
- Attacker Laptop
    

Note: All of these components are hosted on Vultr using a Virtual Private Cloud.

**Private Network:**

- Network: 172.31.0.0/24
    
- IP Range: 172.31.0.1–254
    
- Subnet Mask: 255.255.255.0
    

Note: This initial diagram is a draft and may change as the project progresses.

---

## Scenario Overview

Over the next 30 days, the project will build out a full SOC lab. It will start with the logical diagram, move into environment setup, configuration, attacks, detection, investigations, dashboards, and ticketing integration. Six servers will be used in total: Elastic/Kibana, Windows, Ubuntu, C2, Fleet, and osTicket.

---

## Steps to Create the Logical Diagram

- Go to draw.io and create a new diagram.
    
- Rename the default file to something relevant.
    
- Search for the “Traditional Server Desktop” shape and duplicate it to create six servers.
    
- Visualize the servers inside a Virtual Private Cloud to group them in one private network.
    
- Create links from the Windows and Ubuntu servers to Elastic/Kibana. This represents log forwarding.
    
- Specify the network range: 172.31.0.0/24 with usable IPs from .1 to .254.
    
- Add an internet gateway using a cloud icon to represent the ISP.
    
- Add the Analyst Laptop, which connects to the internet to access Kibana through a browser.
    
- Add the Attacker Laptop and connect it along with the Mythic C2 server to the internet.
    

---

## Component Breakdown

**Elastic/Kibana**  
Kibana is an open-source visualization and exploration tool that is part of the Elastic Stack (ELK Stack), which includes Elasticsearch, Logstash, and Kibana. It allows real-time analysis of data collected from different sources and provides dashboards and visualizations.

**Windows Server**  
A server operating system used to simulate a corporate environment. It provides Active Directory and centralized management.

**Ubuntu Server**  
A Linux-based server used for hosting services and generating logs.

**C2 Server (Mythic)**  
An open-source command and control framework used to simulate attacks in a controlled environment.

**Fleet Server**  
A component of the Elastic stack used to manage and deploy Elastic agents to collect endpoint data.

**osTicket Server**  
An open-source ticketing system used for managing alerts and incident response processes.

---

## Network Summary

The logical diagram shows all components connected inside a private network hosted on Vultr, with an internet gateway for external access.

- Analyst Laptop → Internet → Kibana GUI
    
- Attacker Laptop and Mythic C2 → Internet → Target servers
    
- Windows and Ubuntu servers → Elastic/Kibana for log forwarding
    
- All systems are part of the 172.31.0.0/24 network
    

This completes the Day 1 setup. The logical diagram now acts as the reference architecture for the SOC lab.


## Diagram 


<img width="1001" height="961" alt="image" src="https://github.com/user-attachments/assets/30fba44e-6b3d-4bce-9fa7-6d0c3674355d" />







**Reference**
https://youtu.be/VAE3aVZi0Go?si=vOthmbg_Sb4dkeaF



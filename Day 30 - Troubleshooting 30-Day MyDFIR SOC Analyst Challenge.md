### 1. Introduction

**Goal:** Troubleshoot and finalize the SOC home lab setup, ensuring all components are functional and integrated.

On the final day, the main objective was to validate that all systems, from the ELK stack to the Fleet server, Elastic Agents, and osTicket, were operating correctly. This included resolving any connection issues, verifying service statuses, and confirming that telemetry and alerting mechanisms were working as expected. Ensuring this functionality was critical for the lab to simulate real-world SOC operations effectively.

---

### 2. Troubleshooting Connection Issues

**Steps:**

**Check Firewall Rules:**

- Confirmed that the firewall rules on the Vultr instance allowed all necessary ports for proper operation. This included verifying a broad range of ports (1–65535) for administrative access and testing connectivity from external IP addresses.
    
- Open ports are critical for agent enrollment, communication with Fleet, and telemetry ingestion into Elasticsearch.
    

**Modify Firewall on Virtual Machine:**

- On the Ubuntu VM, the firewall (`ufw`) was configured to allow specific services. For example, port **5601** was opened to allow Kibana access.
    
- Restricting ports prevents unauthorized access while ensuring the services needed for lab functionality are reachable.
    

**Verify System Services:**

- The statuses of **Kibana** and **Elasticsearch** were checked using commands:
    
    `systemctl status kibana.service 
    `systemctl status elasticsearch.service`
    
- Confirming service statuses ensured that critical components were running and that there were no underlying issues affecting connectivity or telemetry ingestion.
    

---

### 3. Configuring Fleet Server

**Steps:**

**Allow Necessary Ports:**

- Ports for Fleet server communication were explicitly opened (e.g., 8220 and 443) to enable agents to connect securely.
    
- Correct port configuration is essential for centralized management of Elastic Agents and seamless telemetry flow.
    

**Update Fleet Server Settings:**

- Within the Elastic web GUI, Fleet settings were adjusted to match the open ports. For instance, changing the default port from **443 to 8220** ensured alignment with firewall rules and agent configuration.
    
- Proper configuration prevents enrollment errors and ensures agents can communicate reliably with the Fleet server.
    

---

### 4. Enrolling Elastic Agent

**Steps:**

**Run Elastic Agent Install Command:**

- From the Elastic Agent directory on each endpoint, the command:
    
    `sudo ./elastic-agent install`
    
    was executed to enroll the agents to the Fleet server.
    

**Troubleshoot Enrollment Issues:**

- Network connectivity was checked whenever enrollment failed, confirming that the Fleet server was accessible.
    
- In cases of self-signed certificate errors, the `--insecure` flag was used to bypass TLS verification temporarily.
    
- Ensured that agents appeared in the Fleet console as healthy and connected, confirming telemetry flow from endpoints to ELK.
    

---

### 5. Finalizing Setup

**Steps:**

**Verify Enrollment:**

- Agents were verified to be successfully enrolled in the Fleet server interface, ensuring continuous log collection and endpoint monitoring.
    

**Test Connectivity:**

- Network connectivity was validated by pinging the Fleet server from endpoints:
    
    `ping <Fleet_Server_IP>`
    
- Confirmed that endpoints could communicate bi-directionally with the server, which is vital for alert triggering and agent management.
    

**Check for Errors:**

- All error messages, such as connection refusals, were reviewed and resolved. These errors typically indicate misconfigured firewall rules, incorrect ports, or agent misconfiguration.
    

---

### 6. Configuring PHP MyAdmin

**Steps:**

**Authorize Public IP Address:**

- Updated PHP MyAdmin user account settings to allow connections via the public IP.
    
- The root account password was set to **Winter2024!** to ensure secure access.
    

**Update Configuration File:**

- Edited `config.inc.php` to reflect the new IP address and credentials.
    
- Verified the configuration by reconnecting to PHP MyAdmin, confirming successful access to the database server.
    

---

### 7. Creating Database for OS Ticket

**Steps:**

**Create Database:**

- A new database for osTicket, e.g., `Challenge-30day-db`, was created in PHP MyAdmin.
    

**Set Privileges:**

- Ensured the root account had proper privileges to access and modify the new database.
    

**Run OS Ticket Installer:**

- Completed the osTicket setup by providing database details and verifying that the web-based ticketing system could receive and process alerts from ELK.
    

---

### 8. Troubleshooting Web Hook Errors

**Steps:**

**Check Network Connection:**

- Confirmed that the ELK server could communicate with the osTicket server using network tests, e.g., pinging the osTicket server from the ELK server.
    

**Update Configuration:**

- Switched osTicket configuration to use the private IP address of the server to avoid firewall or NAT issues.
    
- Tested webhook functionality to confirm alerts triggered tickets correctly, completing the integration and ensuring auditability.

**Reference**
https://youtu.be/o-eR-tJlbqE?si=peve6ZeF9vCeeq8k
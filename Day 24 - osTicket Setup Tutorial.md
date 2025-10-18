
### 1. Introduction

Goal: Set up and configure osTicket, an open-source ticketing system, on a Windows server for the challenge.

---

### 2. Deploying the Server

**Steps:**

1. Log in to Vultr.
2. Click **Deploy New Server**.
3. Select:
    
    - **Cloud Compute** with shared CPU.
    - **Location:** Singapore.
    - **OS:** Windows Standard 2022.
    - **Plan:** 55 GB, 1 CPU, 2 GB RAM.
    - Disable IPv6 and auto backup.
    - Enable **Virtual Private Cloud**.
    - Hostname: `MYDFIR-osTicket`.
    
4. Click **Deploy Now**.
5. After setup, select **View Console**.
6. RDP into the server using:
    
    `xfreerdp /v:<MYDFIR-osTicket-IP> /u:Administrator`
    
![[Screenshot 2025-10-18 214900.png]]
---

### 3. Configuring the Firewall

**Steps:**

1. Go to **Settings → Firewall**.
2. Select **30-Day-MyDFIR-SOC-Challenge Firewall** to restrict inbound access to the web server.
3. Ensure only required ports (80 and 443) are allowed.


---

### 4. Installing XAMPP

**Steps:**

1. Download XAMPP (version 8.2.2 or latest) from Apache Friends.
2. Run the installer and follow the wizard.
3. Default install path:
    
    `C:\xampp`
    
4. Complete installation and launch the XAMPP Control Panel.

![[Pasted image 20251018215231.png]]

---

### 5. Configuring XAMPP

**Steps:**

1. Edit the properties file in:
    
    `C:\xampp`

    - Change `apache_domainname` to:
        
        `<MYDFIR-osTicket public IP>`
        
    - Save the file.
    
![[Pasted image 20251018215406.png]]

2. **Create Firewall Rules**
    
    - Open **Windows Defender Firewall with Advanced Security**.
    - Add inbound rule for:
        
        - Port 80 (HTTP)
        - Port 443 (HTTPS)
            
    - Allow TCP traffic.

![[Pasted image 20251018215730.png]]


---

### 6. Starting XAMPP Services

**Steps:**

1. In XAMPP Control Panel, start:
    
    - **Apache**
    - **MySQL**
        
2. Click **Admin** next to Apache.
3. Open`localhost / 127.0.0.1 | phpMyAdmin 5.2.1` or select `phpMyAdmin` at the top.
    
![[Pasted image 20251018215855.png]]



---

### 7. Configuring PHP MyAdmin

**Steps:**

1. **Authorize Public IP Address**
    
    - Go to **User Accounts**.
    - Edit the **root** account → change hostname from `localhost` to the **public IP**.
    - Set password to:
        `Winter2024!`
        
    - Repeat the same for the **pma** account.

![[Pasted image 20251018220535.png]]



2. **Edit Config File**
    
    - Go to:
        
        `C:\xampp\phpMyAdmin\config.inc.php`
        
    - Backup the file.
        
    - Change:
        
        - Server host → `MYDFIR-osTicket` public IP.
        - Root and pma passwords → `Winter2024!`.
            
    - Save changes.
        
![[Pasted image 20251018220954.png]]

---

### 8. Installing OS Ticket

**Steps:**

1. Download osTicket self-hosted version.
2. Extract files twice to access the `osTicket-v1.18.2` folder.
3. Create a new directory called "osticket":
    
    `C:\xampp\htdocs\osticket`
    
4. Copy the extracted files into the new `osticket` directory.
5. Access:
    
    `http://<MYDFIR-osTicket-IP>/osticket/upload`
    
6. Navigate to:
    
    `C:\xampp\htdocs\osticket\upload\include`
    
    - Rename:
        `ost-sampleconfig.php → ost-config.php`
        

![[Pasted image 20251018221535.png]]


---

### 9. Setting Up OS Ticket

**Steps:**

1. **Create Database**
    
    - Before we proceed with basic installation, we will first create a database on our server.
    - Go to User accounts and select the root account with the public ip address as the host name. Select the Database tab and add privileges to the newly created database on this account.
    
    ![[Pasted image 20251018222327.png]]
![[Pasted image 20251018222611.png]]


2. **Basic Installation**
    
    - Enter Helpdesk Name, Admin credentials.
    
3. **Database Configuration**
    
    - Host: `public IP`
    - Username: `root`
    - Password: `Winter2024!`
        
4. **Complete Installation**
    
    - Ensure DB privileges are set.
    - Rerun setup if required until successful.

---

### 10. Finalizing OS Ticket Setup

**Steps:**

1. **Configure File Permissions**
    
    - Navigate to:
        
        `C:\xampp\htdocs\osticket\upload\include`
        
    - Open PowerShell as Administrator.
        
    - Run:
        
        `icacls .\ost-config.php /reset`
        
2. **Access URLs**
    
    - Client Portal:
        
        `http://<Challenge-OSticket-IP>/osticket/upload/`
        
    - Staff Panel:
        
        `http://<Challenge-OSticket-IP>/osticket/upload/scp`
        
3. Log in using the admin credentials created during setup.
    

---

### 11. Exploring OS Ticket

**Steps:**

- **Admin Panel**
    
    - Configure helpdesk settings.
    - Create new agent accounts.
    - Adjust title and branding.
    
- **Ticket Management**
    
    - View, assign, and manage tickets.
    - Test ticket creation and response flow.
        

---

### 12. Conclusion

- Successfully deployed a Windows server and installed XAMPP.
- Configured PHP MyAdmin for remote access.
- Installed and configured osTicket for ticket management.
- Accessed both client and staff portals.
- Verified ticketing system functionality.

**Reference**
https://youtu.be/xgxQuLL33oU?si=rNBpLYiRwek7Ftym
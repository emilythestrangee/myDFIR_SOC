
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
    
<img width="730" height="576" alt="image" src="https://github.com/user-attachments/assets/baf8dca1-d60a-4ddb-a30f-8764382f05d4" />

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

<img width="842" height="603" alt="image" src="https://github.com/user-attachments/assets/537d6161-bf1d-4d7d-87d0-fcad0d793dc9" />


---

### 5. Configuring XAMPP

**Steps:**

1. Edit the properties file in:
    
    `C:\xampp`

    - Change `apache_domainname` to:
        
        `<MYDFIR-osTicket public IP>`
        
    - Save the file.
    
<img width="1100" height="583" alt="image" src="https://github.com/user-attachments/assets/74ded6fe-e42f-4395-b2fb-673a03a8ca18" />


2. **Create Firewall Rules**
    
    - Open **Windows Defender Firewall with Advanced Security**.
    - Add inbound rule for:
        
        - Port 80 (HTTP)
        - Port 443 (HTTPS)
            
    - Allow TCP traffic.

<img width="1045" height="718" alt="image" src="https://github.com/user-attachments/assets/2c5f0df8-6ce1-4b29-9ad7-9fb5949463e9" />



---

### 6. Starting XAMPP Services

**Steps:**

1. In XAMPP Control Panel, start:
    
    - **Apache**
    - **MySQL**
        
2. Click **Admin** next to Apache.
3. Open`localhost / 127.0.0.1 | phpMyAdmin 5.2.1` or select `phpMyAdmin` at the top.
    
<img width="680" height="445" alt="image" src="https://github.com/user-attachments/assets/cf7fdc5c-ba19-4e1f-826a-bcdbdd9cf979" />



---

### 7. Configuring PHP MyAdmin

**Steps:**

1. **Authorize Public IP Address**
    
    - Go to **User Accounts**.
    - Edit the **root** account → change hostname from `localhost` to the **public IP**.
    - Set password to:
        `Winter2024!`
        
    - Repeat the same for the **pma** account.

<img width="1100" height="530" alt="image" src="https://github.com/user-attachments/assets/edb7751b-efe9-49dc-be0b-d9e80027e7b9" />




2. **Edit Config File**
    
    - Go to:
        
        `C:\xampp\phpMyAdmin\config.inc.php`
        
    - Backup the file.
        
    - Change:
        
        - Server host → `MYDFIR-osTicket` public IP.
        - Root and pma passwords → `Winter2024!`.
            
    - Save changes.
        
<img width="1100" height="590" alt="image" src="https://github.com/user-attachments/assets/7d37483b-85c8-46ac-b566-bb3c1d9b459c" />


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
        

<img width="595" height="719" alt="image" src="https://github.com/user-attachments/assets/57b4525f-9567-462f-b0f0-5244a1da19d6" />



---

### 9. Setting Up OS Ticket

**Steps:**

1. **Create Database**
    
    - Before we proceed with basic installation, we will first create a database on our server.
    - Go to User accounts and select the root account with the public ip address as the host name. Select the Database tab and add privileges to the newly created database on this account.
    
    <img width="517" height="388" alt="image" src="https://github.com/user-attachments/assets/6aacb76b-2642-4949-a70b-c86eb0511d3f" />

<img width="825" height="535" alt="image" src="https://github.com/user-attachments/assets/5b1049fb-e614-4279-ab3a-dfbc702e4b2a" />



2. **Basic Installation**
    
    - Enter Helpdesk Name, Admin credentials.
    
3. **Database Configuration**
    
    - Host: `public IP`
    - Username: `root`
    - Password: `Winter2024!`
        
4. **Complete Installation**
    
    - Ensure DB privileges are set.
    - Rerun setup if required until successful.
    
<img width="1100" height="532" alt="image" src="https://github.com/user-attachments/assets/e3dc9e92-e7bf-4ff0-a450-dd133b7165b5" />


<img width="563" height="459" alt="image" src="https://github.com/user-attachments/assets/cedba148-9b6b-429c-963d-ff54c6f10a3d" />


---

### 10. Finalizing OS Ticket Setup

**Steps:**

1. **Configure File Permissions**
    
    - Navigate to:
        
        `C:\xampp\htdocs\osticket\upload\include`
        
    - Open PowerShell as Administrator.
    - Run:
        
        `icacls .\ost-config.php /reset`
    
<img width="697" height="123" alt="image" src="https://github.com/user-attachments/assets/fccc6208-c03c-45f3-99d7-da691d979134" />



2. **Access URLs**
    
    - Client Portal:
        
        `http://<MYDFIR-osTicket-IP>/osticket/upload/`

<img width="825" height="421" alt="image" src="https://github.com/user-attachments/assets/f94791a1-ad9e-46f9-854c-0205177bb19b" />



    - Staff Panel:
        
        `http://<MYDFIR-osTicket-IP>/osticket/upload/scp`

<img width="707" height="246" alt="image" src="https://github.com/user-attachments/assets/0837cb36-4580-4191-8e8a-3c3e82ac07b8" />

3. Log in using the admin credentials created during setup (Username: **MyDFIR**).


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

<img width="537" height="212" alt="image" src="https://github.com/user-attachments/assets/af11754c-49ea-4920-8d10-5c5823641b50" />


---

### 12. Conclusion

- Successfully deployed a Windows server and installed XAMPP.
- Configured PHP MyAdmin for remote access.
- Installed and configured osTicket for ticket management.
- Accessed both client and staff portals.
- Verified ticketing system functionality.

**Reference**
https://youtu.be/xgxQuLL33oU?si=rNBpLYiRwek7Ftym


### 1. Introduction

**Goal:** Successfully integrate osTicket with the ELK Stack to automate ticket creation from alerts and send a test alert into osTicket.

---

### 2. Creating an API Key in OS Ticket

**Steps:**

1. **Access OS Ticket Admin Panel**
    
    - Navigate to:
        
        `http://<MYDFIR-osTicket-IP>/osticket/upload/scp`
        
    - Log in as an administrator.
        
2. **Add a New API Key**
    
    - Click **Admin Panel** on the top menu.
    - Go to **Manage → API**.
    - Click **Add New API Key**.
    
3. **Configure the Key**
    
    - Enter the **private IP address** of your Challenge-ELK server (since both are in the same VPC).
    - Check **Can Create Tickets**.
    - Add internal notes.
    - Click **Add Key**.
    
4. **Save the Key**
    
    - Copy and store the generated API key securely, it will be used in Elastic.
    

![[Pasted image 20251018225806.png]]

---

### 3. Configuring Elastic Connector

**Steps:**

1. **Enable Free 30-Day Trial** _(Required to use Webhook Connectors)_
    
    - Log in to Elastic Stack GUI.
    - Click on the **hamburger icon → Stack Management**.
    - Go to **License Management**.
    - Start the 30-day trial.
    
2. **Create a Webhook Connector**
    
    - In **Stack Management**, go to **Connectors** under _Alerts and Insights_.
    - Click **Create Connector**.
    - Select **Webhook**.
    - Fill in:
        
        - **Name:** `osTicket`
        - **URL:**
            `http://<OS_Ticket_Private_IP>/osticket/upload/api/tickets.xml`
            
        - **Method:** POST
        - **Authentication:** None
        - **HTTP Header:**
            
            - Key: `X-API-Key`
            - Value: `_<your API key>_`
                
    - Click **Save & Test**.
        
![[Pasted image 20251018231505.png]]

---
### 4. Creating Action Body (Payload)

**Steps:**

1. **Get XML Payload Example**
    
    - From osTicket GitHub documentation:  
        [osTicket API tickets.md](https://github.com/osTicket/osTicket/blob/develop/setup/doc/api/tickets.md)
        
2. **Sample XML Payload:**

    
    ![[Pasted image 20251018232447.png]]
3. **Customize Payload**
    
    - Paste the XML payload into the connector’s **Action Body** field.
    - Update the subject line to something meaningful (e.g., MyDFIR-30DayChallenge-emaan).
        
4. **Run the Test**
    
    - Click **Run Test**.
    - If successful, a ticket should be created in osTicket.


![[Pasted image 20251018232512.png]]

---

### 5. Troubleshooting Connection Issues

**Steps:**

1. **Check Network Reachability**
    
    - If the test fails with _“Unreachable”_, check the connection between ELK and osTicket.
    - SSH into your ELK server and:
        
        `ping <OS_Ticket_Private_IP>`
        
2. **Verify IP Configuration**
    
    - Ensure the osTicket server has its **private VPC IP** properly set.
        
    - Update network adapter settings if necessary.
        
3. **Update Connector URL**
    
    - Use the private IP address instead of the public one if in the same VPC.
        
    - Save and re-run the test.
        
4. **Firewall Rules**
    
    - Ensure ports 80 (HTTP) or 443 (HTTPS) are allowed from ELK to osTicket.
        

---

### 6. Confirming the Integration

**Steps:**

1. Log back into osTicket Agent Panel:
    
    `http://<Challenge-OSticket-IP>/osticket/upload/scp`
    
2. Go to **Tickets**.
    
3. Verify that a new ticket has been created from the webhook test.
    
4. Confirm that the subject and message match the XML payload details.
    

---

### 7. Conclusion

- Created and configured an API key in osTicket.
- Enabled Elastic’s 30-day trial to use Webhook connectors.
- Configured a Webhook connector in Elastic with proper headers and payload.
- Troubleshot network connectivity and verified successful integration.
- Confirmed that alerts in Elastic can now automatically generate tickets in osTicket.

This integration streamlines alert tracking and documentation, ensuring security events are accounted for and properly managed.

**Reference**
https://youtu.be/P9YxutqWAF0?si=3D_WKyhhvN5mfTBe
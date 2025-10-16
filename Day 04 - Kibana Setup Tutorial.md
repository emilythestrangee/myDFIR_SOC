
## Objective

Set up and configure a **Kibana** instance to integrate with the Elasticsearch service. This includes downloading and installing the package, editing its configuration, setting up secure enrollment, and enabling encryption keys.

---

## Why Kibana?

Kibana is the visualization and interface layer of the ELK stack. It allows analysts to explore data from Elasticsearch using dashboards, queries, and alerting features. By the end of this setup, you’ll be able to access Kibana remotely and use it to visualize security logs.

---

## Step 1 — Download Kibana

SSH into the Ubuntu instance where Elasticsearch is installed. Get the download link from the official Elastic website, then run:

`wget https://artifacts.elastic.co/downloads/kibana/kibana-9.1.5-amd64.deb

Check if the package downloaded correctly:

`ls`

---

## Step 2 — Install Kibana

Install the downloaded `.deb` file:

`dpkg -i kibana-9.1.5-amd64.deb`

---

## Step 3 — Configure Kibana

Open the configuration file:

`nano /etc/kibana/kibana.yml`

Uncomment and modify the following lines:

`server.port: 5601 server.host: 207.148.79.228

Save the file and exit.
<img width="608" height="203" alt="image" src="https://github.com/user-attachments/assets/17a7df8a-eb16-47b3-aebf-43f526b19766" />


---

## Step 4 — Start the Kibana Service

Reload and enable the service. Check to make sure the status shows as “active (running).

<img width="508" height="117" alt="image" src="https://github.com/user-attachments/assets/8f4e87c4-cf7b-4593-89db-28e9d84532e5" />


---

## Step 5 — Generate Enrollment Token

Kibana uses an enrollment token to establish a secure connection with Elasticsearch.

Navigate to the Elasticsearch binary directory:

`cd /usr/share/elasticsearch/bin`

Generate the token:

`./elasticsearch-create-enrollment-token --scope kibana`

Copy and save the token — you’ll use it shortly in the browser.
<img width="1108" height="198" alt="image" src="https://github.com/user-attachments/assets/faabf2e6-26ff-431d-8769-9b85fa52b4f2" />


---

## Step 6 — Access Kibana Web Interface

Before opening the browser, allow traffic to port 5601 by configuring the firewall rules on vultr as well as the VM. We need to allow access from our public IP to that port, since we previously made it possible just for port 22 to be granted access. Head back to Compute →Click on your instance → Settings → Firewall → Manage: 
<img width="348" height="261" alt="image" src="https://github.com/user-attachments/assets/d4b8f535-11e1-4707-89fa-a008a9f876f8" />



<img width="355" height="64" alt="image" src="https://github.com/user-attachments/assets/f2cb0ea7-f9df-47dd-9287-d42028313e87" />


Also, update any cloud firewall rules to permit traffic on port 5601 from your IP.Then open a browser and go to:

`http://207.148.79.228:5601`

If the service is accessible, you’ll see the enrollment page:
<img width="1100" height="547" alt="image" src="https://github.com/user-attachments/assets/8cf44c73-d69c-46db-80a1-5d98638c0e98" />


---

## Step 7 — Enrollment and Login

Paste the enrollment token into the enrollment field. Then, in your server terminal, generate the verification code:

`cd /usr/share/kibana/bin ./kibana-verification-code`
<img width="1015" height="71" alt="image" src="https://github.com/user-attachments/assets/d04dcab7-e037-440b-bcdb-a2bcaa9f3cb2" />



Enter the verification code into the web interface. Once verified, you’ll be redirected to the login page.

- **Username**: `elastic`
    
- **Password**: retrieved from the Elasticsearch installation log or reset with `/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic`:
<img width="1100" height="421" alt="image" src="https://github.com/user-attachments/assets/7f3c458c-cc48-49f1-84a6-3337111dfda4" />

    

Log in to the dashboard.
<img width="926" height="758" alt="image" src="https://github.com/user-attachments/assets/b3e57cc7-1e6a-43ed-a58d-4923d44401c4" />

<img width="1100" height="544" alt="image" src="https://github.com/user-attachments/assets/89c9ea9c-e221-4d1b-9666-f5fd8d8ae904" />

---

## Step 8 — Configure Encryption Keys

Encryption keys are required to secure saved objects, reporting, and security features in Kibana. Navigate to the Kibana binary directory and generate the keys:

`./kibana-encryption-keys generate`

<img width="1080" height="135" alt="image" src="https://github.com/user-attachments/assets/ffeddc30-e8e3-4465-97dc-992ff4eeb7f6" />



You’ll receive three keys. Add each to the Kibana keystore. Paste the appropriate values when prompted and restart Kibana to apply the changes:

<img width="845" height="168" alt="image" src="https://github.com/user-attachments/assets/2452c0a0-2079-4687-a102-ee2c8eec4629" />



Log in again to confirm the configuration works correctly.

Before generating encryption keys:
<img width="1100" height="351" alt="image" src="https://github.com/user-attachments/assets/09217675-64ee-4107-9c38-0ebdd8ae37be" />



After generating encryption keys:
<img width="1100" height="444" alt="image" src="https://github.com/user-attachments/assets/31b09571-943e-40f1-86c5-75a92aacf0ef" />


---

## Summary

- Installed Kibana alongside Elasticsearch.
    
- Configured the host to allow external access.
    
- Used the enrollment token and verification code to securely connect Kibana with Elasticsearch.
    
- Added encryption keys to secure Kibana’s sensitive data and restarted the service.
    
- Verified the web interface is accessible.
    

Kibana is now operational and ready to be integrated into the SOC environment.

**Reference**
https://youtu.be/tXwMoBbrkYw?si=faa3yxMShagVz6zO

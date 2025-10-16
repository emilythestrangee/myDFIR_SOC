
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
![[Pasted image 20251016222645.png]]

---

## Step 4 — Start the Kibana Service

Reload and enable the service. Check to make sure the status shows as “active (running).

![[Pasted image 20251016222822.png]]

---

## Step 5 — Generate Enrollment Token

Kibana uses an enrollment token to establish a secure connection with Elasticsearch.

Navigate to the Elasticsearch binary directory:

`cd /usr/share/elasticsearch/bin`

Generate the token:

`./elasticsearch-create-enrollment-token --scope kibana`

Copy and save the token — you’ll use it shortly in the browser.
![[Pasted image 20251016223000.png]]

---

## Step 6 — Access Kibana Web Interface

Before opening the browser, allow traffic to port 5601. We need to allow access from our public IP to that port, since we previously made it possible just for port 22 to be granted access. Head back to Compute →Click on your instance → settings → firewall → manage: 

`ufw allow 5601`

Also, update any cloud firewall rules to permit traffic on port 5601 from your IP.

Then open a browser and go to:

`http://207.148.79.228:5601`

If the service is accessible, you’ll see the enrollment page.

---

## Step 7 — Enrollment and Login

Paste the enrollment token into the enrollment field.

Then, in your server terminal, generate the verification code:

`cd /usr/share/kibana/bin ./kibana-verification-code`

Enter the verification code into the web interface.

Once verified, you’ll be redirected to the login page.

- **Username**: `elastic`
    
- **Password**: retrieved from the Elasticsearch installation log or reset with:
    

`/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic`

Log in to the dashboard.

---

## Step 8 — Configure Encryption Keys

Encryption keys are required to secure saved objects, reporting, and security features in Kibana.

Generate the keys:

`./kibana-encryption-keys generate`

You’ll receive three keys. Add each to the Kibana keystore:

`./kibana-keystore add xpack.encryptedSavedObjects.encryptionKey ./kibana-keystore add xpack.reporting.encryptionKey ./kibana-keystore add xpack.security.encryptionKey`

Paste the appropriate values when prompted.

Restart Kibana to apply the changes:

`systemctl restart kibana`

Log in again to confirm the configuration works correctly.

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
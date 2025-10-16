
## Introduction

Goal: Ingest Sysmon and Windows Defender event logs from the Windows Server into Elasticsearch and confirm log visibility in Kibana.

---

## Log into Elasticsearch

Open your Elasticsearch instance and click the **Add Integrations** button on the homepage. You can also find this option in the top-left menu.
<img width="1100" height="657" alt="image" src="https://github.com/user-attachments/assets/35eebe8a-b21f-4cd9-84ea-0b742c27089c" />


---

## Add Sysmon Integration

Search for "Windows" and select **Custom Windows Event Logs**. Click **Add Custom Windows Event Logs**.
<img width="1100" height="545" alt="image" src="https://github.com/user-attachments/assets/c4529e89-8df0-4dd8-80ca-efd393d61fa3" />


Provide a name for the integration and a description.
<img width="689" height="259" alt="image" src="https://github.com/user-attachments/assets/6e6c9067-3f48-4f7e-9e07-3ca900633a67" />


Obtain the **Channel Name** from the Windows Server:

- RDP into the server and open **Event Viewer**.
    
- Navigate to:
`Applications and Services Logs → Microsoft → Windows → Sysmon → Operational`

- Right-click **Operational**, select **Properties**, and copy the full name:
`Microsoft-Windows-Sysmon/Operational`

Paste this full name into the integration setup. Add the integration to an existing host and select the agent policy (`MyDFIR-Windows-Policy`). Click **Save and Continue**, then **Save and Deploy Changes**.

<img width="1100" height="219" alt="image" src="https://github.com/user-attachments/assets/f224c7b6-7f1a-42ac-9930-0ff2ec372919" />

---

## Add Windows Defender Integration

On the same **Custom Windows Event Logs** page, click **Add Custom Windows Event Logs** for Windows Defender.

Provide a name and a description:
<img width="647" height="223" alt="image" src="https://github.com/user-attachments/assets/a858106f-3165-4ccc-94d3-0a4ea8cdfd11" />


Obtain the **Channel Name**:

- Navigate to:
`Applications and Services Logs → Microsoft → Windows → Windows Defender → Operational`

- Right-click **Operational**, select **Properties**, and copy the full name:
`Microsoft-Windows-Windows Defender/Operational`

Paste this into the integration setup. Click **Advanced Options** and specify the Event IDs to include:

`1116, 1117, 5001`

<img width="463" height="358" alt="image" src="https://github.com/user-attachments/assets/a51d7379-b666-4231-b44b-5dd165e56895" />


Add the integration to the same existing host and agent policy MyDFIR-Windows-Policy. Click **Save and Continue**, then **Save and Deploy Changes**.

---

## Verify Log Ingestion

Open the **Discover** tab in Kibana and filter for `winlog.event_id` or search for “sysmon”.

If logs do not appear:

- Ensure the integration is assigned to the correct host and agent policy.
- Restart the Elastic Agent on the Windows Server:
`Restart-Service -Name "Elastic Agent"`

- Confirm incoming connections to port 9200 on the Elasticsearch instance.
<img width="504" height="249" alt="image" src="https://github.com/user-attachments/assets/0e095df2-d8bc-4a28-add9-c338129ab9a4" />


- Refresh Kibana and check for Sysmon logs (winlog.event_id:1):

<img width="890" height="676" alt="image" src="https://github.com/user-attachments/assets/dede485c-1a59-4fc4-9108-611dc61917ae" />

<img width="476" height="301" alt="image" src="https://github.com/user-attachments/assets/34e83b31-4440-4faf-befc-e53d8494b222" />


## Generating Windows Defender Logs for Testing

If Defender logs are not immediately visible, trigger an event:

- Open **Windows Security** → **Virus & threat protection**.
- Temporarily turn off real-time protection, then turn it back on.
- The corresponding log should appear in Kibana (event code `5001`).

<img width="562" height="499" alt="image" src="https://github.com/user-attachments/assets/6dc3db7c-165c-4d48-abab-98e95f580454" />

<img width="438" height="274" alt="image" src="https://github.com/user-attachments/assets/bca6af14-171b-47e3-970f-e087f4cf80ec" />

---

## Notes

- Only relevant Event IDs are ingested to avoid noise.
    
- Sysmon logs can be verified using Event ID 1 (process creation).
    
- Windows Defender logs of interest:
    
    - 1116: Malware detected
        
    - 1117: Malware action taken
        
    - 5001: Real-time protection disabled
        

With this setup, both Sysmon and Windows Defender logs are successfully integrated and ingested into Elasticsearch, ready for monitoring and analysis.

**Reference**
https://youtu.be/eOie0SDMuGA?si=RJCGWgX2AK2OhRvx

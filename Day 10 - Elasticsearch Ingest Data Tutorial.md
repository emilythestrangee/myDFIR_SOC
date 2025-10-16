
## Introduction

Goal: Ingest Sysmon and Windows Defender event logs from the Windows Server into Elasticsearch and confirm log visibility in Kibana.

---

## Log into Elasticsearch

Open your Elasticsearch instance and click the **Add Integrations** button on the homepage. You can also find this option in the top-left menu.
![[Pasted image 20251017012617.png]]

---

## Add Sysmon Integration

Search for "Windows" and select **Custom Windows Event Logs**. Click **Add Custom Windows Event Logs**.
![[Pasted image 20251017012815.png]]

Provide a name for the integration and a description.
![[Pasted image 20251017013107.png]]

Obtain the **Channel Name** from the Windows Server:

- RDP into the server and open **Event Viewer**.
    
- Navigate to:
`Applications and Services Logs → Microsoft → Windows → Sysmon → Operational`

- Right-click **Operational**, select **Properties**, and copy the full name:
`Microsoft-Windows-Sysmon/Operational`

Paste this full name into the integration setup. Add the integration to an existing host and select the agent policy (e.g., `Challenge Windows Policy`). Click **Save and Continue**, then **Save and Deploy Changes**.

---

## Add Windows Defender Integration

On the same **Custom Windows Event Logs** page, click **Add Custom Windows Event Logs** for Windows Defender.

Provide a name (e.g., `Challenge-Win-Defender`) and a description (e.g., “Collect Windows Defender logs for Event IDs 1116, 1117 & 5001”).

Obtain the **Channel Name**:

- Navigate to:
    

`Applications and Services Logs → Microsoft → Windows → Windows Defender → Operational`

- Right-click **Operational**, select **Properties**, and copy the full name:
    

`Microsoft-Windows-Windows Defender/Operational`

Paste this into the integration setup. Click **Advanced Options** and specify the Event IDs to include:

`1116, 1117, 5001`

Add the integration to the same existing host and agent policy. Click **Save and Continue**, then **Save and Deploy Changes**.

---

## Verify Log Ingestion

Open the **Discover** tab in Kibana and filter for `winlog.event_id` or search for “sysmon”.

If logs do not appear:

- Ensure the integration is assigned to the correct host and agent policy.
    
- Restart the Elastic Agent on the Windows Server:
    

`Restart-Service -Name "Elastic Agent"`

- Confirm incoming connections to port 9200 on the Elasticsearch instance.
    
- Refresh Kibana and check for logs.
    

---

## Generating Windows Defender Logs for Testing

If Defender logs are not immediately visible, trigger an event:

- Open **Windows Security** → **Virus & threat protection**.
    
- Temporarily turn off real-time protection, then turn it back on.
    
- The corresponding log should appear in Kibana (event code `5001`).
    

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
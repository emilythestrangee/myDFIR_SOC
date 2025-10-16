## Introduction

Goal: Install Sysmon on a Windows server and verify that it generates logs (telemetry).

---

## Connect to Windows Server

Use Remote Desktop (RDP) to connect to the Windows server MYDFIR-WIN-emaan (IP and credentials provided). Open any available browser on the server to download Sysmon.

---

## Download Sysmon

Google "Sysmon" and select the first link: [Sysmon - Microsoft Docs](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon). Download the Sysmon ZIP file directly from the page.

---

## Extract Sysmon

Navigate to the `Downloads` folder. Right-click the downloaded Sysmon ZIP and select **Extract All**. Confirm that three binaries are extracted in the folder.
<img width="394" height="255" alt="image" src="https://github.com/user-attachments/assets/3a8d3b5d-0b3f-43c7-8051-0fd98063cfed" />

---

## Download Configuration File

Search for "Sysmon Olaf configuration" and select the GitHub repository link: [sysmon-modular](https://github.com/olafhartong/sysmon-modular). Locate `sysmonconfig.xml` in the repository. Click **Raw** and save the file as `sysmonconfig.xml` inside the extracted Sysmon directory.

---

## Open PowerShell

Open PowerShell with Administrator privileges. Navigate to the Sysmon directory using the `cd` command:

`cd "C:\Users\Administrator\Downloads\Sysmon"`

---

## Install Sysmon

Run the following command to install Sysmon with the configuration file:

`.\Sysmon64.exe -i sysmonconfig.xml`

Accept the license agreement if prompted. Confirm that the Sysmon service is running in **Services**. Open Event Viewer and expand **Applications and Services Logs → Microsoft → Windows → Sysmon → Operational** to view log entries.
<img width="600" height="454" alt="image" src="https://github.com/user-attachments/assets/ecdf8f4d-7052-4235-b3bb-2f3c470b917e" />


---

## Next Steps

Integrate Sysmon logs with the ELK stack (Elasticsearch, Logstash, Kibana) for centralized monitoring and analysis. Familiarize yourself with key Sysmon Event IDs to detect suspicious activity effectively.


**Reference**
https://youtu.be/nzZY9OSfkeg?si=A81pXRzT9ho8JPM8

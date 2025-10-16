
## Goal

Deploy a Windows Server 2022 instance to act as a target machine with RDP exposed for lab exercises. Understand how to deploy on a cloud provider, enable RDP access, and why we place this host outside the VPC for segmentation.

---

## Overview / Rationale

- The Windows server is used as a **vulnerable target** for simulated attacks (e.g., brute force).
    
- Intentionally placing it **outside the VPC** reduces blast radius: if the target is compromised, the rest of the internal lab network is not automatically accessible.
    
- Exposing RDP to the internet is risky—do this only in a controlled lab and monitor it closely.
    

---

## Deploying on GCP (summary)

1. Create a VM instance:
    
    - Go to VM Instances → Create Instance.
        
    - Choose a name and the same region as other lab instances.
        
    - Select a small machine type (example: e2-small) and choose **Windows Server 2022** as the image.
        
    
    _img: placeholder for GCP Create Instance screenshot_
    
2. Wait for the VM to provision (several minutes).
    
3. Set a Windows password from the instance page:
    
    - Click the arrow next to the RDP button → Set Windows password.
        
    - Save the generated password securely.
        
    
    _img: placeholder for GCP Set Windows Password screenshot_
    
4. Configure a firewall rule (example for lab):
    
    - Create an ingress rule named e.g. `allow-rdp` with TCP port **3389** and source `0.0.0.0/0` (for lab purposes only).
        
    - Tag the instance (e.g., `win-vuln`) and apply the firewall rule to that tag.
        
    
    _img: placeholder for firewall rule screenshot_
    
5. Connect via Remote Desktop:
    
    - Use the external IP, username, and saved password to RDP from your local machine.
        
    - First-time connections may take a bit longer while the server finalizes setup.
        
    
    _img: placeholder for RDP login screenshot_
    

---

## Deploying on Vultr (summary)

1. Deploy a new server:
    
    - Deploy → Deploy New Server.
        
    - Choose **Cloud Compute (Shared CPU)**, a location matching other resources, and select **Windows Standard 2022** as the image.
        
    - Pick a lightweight plan (e.g., 1 vCPU / 2 GB for lab).
        
    - Disable IPv6 and Auto Backups under Additional Features.
        
    - Do **not** add this VM to the VPC (leave it public for lab exposure).
        
    
    _img: placeholder for Vultr Deploy screenshot_
    
2. Wait until the VM status becomes **Running**, then open **View Console** to see the Windows GUI and sign in using the provided credentials.
    
    _img: placeholder for Vultr Console screenshot_
    
3. Test RDP:
    
    - Copy the public IP and use Remote Desktop from your workstation.
        
    - Enter credentials and verify you can connect.
        
    
    _img: placeholder for RDP connection screenshot_
    

---

## Security considerations (lab vs production)

- Exposing RDP (port 3389) to the internet is insecure for production. For a lab, exposure is intentional but should be controlled:
    
    - Limit exposure time.
        
    - Log and monitor authentication attempts.
        
    - Use separate test credentials and isolated accounts.
        
- In production you should use VPNs, IP whitelisting, or jump boxes; enable MFA; and restrict RDP to trusted networks only.
    

---

## Monitoring and expected behavior

- Once RDP is public, expect automated login attempts from internet scanners—these generate logs you can use for detection exercises.
    
- These failed authentications and brute-force attempts become useful telemetry for ELK and for creating detection rules later in the challenge.
    

---

## Quick checklist

-  Deploy Windows Server in chosen cloud region.
    
-  Ensure VM is **not** added to the lab VPC (for target segmentation).
    
-  Save generated Windows credentials.
    
-  Create/assign firewall rules (for lab: open 3389; in real life: restrict).
    
-  Connect via RDP and confirm access.
    
-  Start monitoring logs for brute-force activity.
**Reference**
https://youtu.be/nBlCuLMq-zA?si=5OLWhNxRuBScCRm6
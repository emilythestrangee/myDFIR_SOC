
## Introduction

Goal: Understand brute force attacks, common tooling, and practical defenses. Brute force remains a frequently used method because it’s easy to automate and can succeed against weak or reused credentials.

---

## What is a brute force attack?

A brute force attack tries many possible credentials until one works. It ranges from systematically testing every combination (e.g., 0000 → 0001 → …) to replaying lists of leaked username/password pairs against services exposed to the internet (RDP, SSH, web login forms).

Because many services are publicly reachable and users often reuse weak passwords, brute force and credential-stuffing attacks are common and effective.

---

## Common types

- **Simple brute force:** Exhaustively tries all possible combinations for a password or PIN. Usually automated and noisy.
    
- **Dictionary attack:** Uses a curated list of likely passwords (common words, leaked passwords, variations). More efficient than blind exhaustive search.
    
- **Credential stuffing:** Uses large dumps of real username/password pairs from breaches and attempts them against other services, relying on password reuse.
    

---

## Typical tools used

- **Hydra** — network login brute forcer (SSH, HTTP, FTP, etc.).
    
- **Hashcat** — high-performance password cracker for hashed passwords.
    
- **John the Ripper** — versatile password cracker supporting many hash types and strategies.
    

(These are the common offensive tools; as an analyst you should be familiar with their fingerprints and the artefacts they leave.)

---

## Protections and mitigations

- **Use long, unique passwords / passphrases.** Aim for 15+ characters or a strong passphrase; complexity helps but length matters most. Use a password manager to generate and store unique credentials.
    
- **Enable Multi-Factor Authentication (MFA).** Prefer authenticator apps or hardware tokens over SMS/email. MFA blocks many brute-force and credential-stuffing attacks even if a password is compromised.
    
- **Rate-limit and lockout policies.** Implement account lockout or progressive delays after failed login attempts to slow automated attacks.
    
- **Expose minimal services.** Identify public-facing services (RDP, SSH, web admin consoles) and reduce the attack surface by disabling unused services or placing them behind VPNs / jump boxes.
    
- **Network and host-level controls.** Use firewalls, geo-blocking where appropriate, and IP reputation lists to reduce malicious login attempts.
    
- **Monitoring and alerting.** Log failed login spikes, repeated attempts from single IPs, and anomalous geolocations; create alerts for patterns consistent with brute-force or credential stuffing.
    
- **Credential hygiene.** Encourage or enforce password changes after breaches, and subscribe to breach-notification services (e.g., Have I Been Pwned) to detect exposed accounts.
    
- **User awareness.** Train users to recognise phishing and social engineering which often enable credential compromise.
    

---

## Additional measures (operational)

- Use web application protections (WAF) for public login endpoints.
    
- Implement MFA enforcement for privileged accounts (administrators, service accounts).
    
- Block or quarantine IPs that show credential-stuffing behaviour and feed them into threat intel lists.
    
- Monitor honeypots or low-value accounts to detect opportunistic credential stuffing.
    

---

## Key takeaways

- Brute force and credential stuffing are low-cost, high-volume attacks that target weak or reused credentials.
    
- The strongest defenses combine authentication hardening (long unique passwords + MFA), exposure reduction (limit public services), and monitoring with automated mitigations (rate limiting, lockouts, IP blocking).
    
- As a SOC analyst, focus on detection (failed login anomalies, unusual sources), rapid containment, and remediation (forcing password resets, MFA rollout) when these attacks are observed.

**Reference**
https://youtu.be/Tv57yhAOb6g?si=fnhe4MLg0N6SSCFo
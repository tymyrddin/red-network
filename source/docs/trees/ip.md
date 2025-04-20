# Network Layer Protocol (IPv4 or IPv6)

BGP runs over IP (Internet Protocol). It can operate over both IPv4 (traditional BGP) and IPv6 (MP-BGP for 
multiprotocol support).

## Compromise Internet Protocol (IP)

```text
1. Initial Access [OR]

    1.1 Phishing & Social Engineering [OR]
    
        1.1.1 Spear Phishing (PDF/Excel malware)
        
        1.1.2 Business Email Compromise (BEC) with deepfake audio/video
        
        1.1.3 LinkedIn/Twitter impersonation for credential theft
        
    1.2 Exploiting Cloud Misconfigurations [OR]
    
        1.2.1 Exposed S3 buckets (AWS) or Azure Blob Storage
        
        1.2.2 Misconfigured GitHub/GitLab repos (API keys, credentials)
        
    1.3 Supply Chain Attacks [OR]
    
        1.3.1 Dependency confusion (malicious npm/PyPi packages) (2021)
        
        1.3.2 Compromised SaaS vendors (SolarWinds-style attacks)

2. Lateral Movement & Privilege Escalation [AND]

    2.1 Exploiting Zero-Day Vulnerabilities [OR]
    
        2.1.1 RCE in enterprise VPNs (Pulse Secure, Citrix CVE-2023-3519)
        
        2.1.2 Windows/Linux privilege escalation (Dirty Pipe, Log4Shell)
        
    2.2 Cloud Identity Attacks [OR]
    
        2.2.1 OAuth token hijacking (Microsoft/Azure AD)
        
        2.2.2 Shadow API abuse (undocumented cloud APIs)

3. Data Exfiltration [AND]

    3.1 Encrypted Exfiltration [OR]
    
        3.1.1 DNS tunneling (DoH/DoT for stealth)
        
        3.1.2 Legitimate cloud services (Dropbox, Google Drive, Slack)
        
    3.2 Insider Threats [OR]
    
        3.2.1 Rogue employees using USB exfiltration (Rubber Ducky attacks)
        
        3.2.2 Compromised contractors with excessive access

4. Persistence & Evasion [OR]

    4.1 Fileless Malware [OR]
    
        4.1.1 PowerShell/Cobalt Strike in-memory execution
        
        4.1.2 Linux rootkits (Symbiote, 2022)
        
    4.2 Cloud Backdoors [AND]
    
        4.2.1 Malicious Lambda functions (AWS)
        
        4.2.2 Hidden service accounts in Google Workspace

5. Counter-Forensics [OR]

    5.1 Log Manipulation [OR]
    
        5.1.1 SIEM poisoning (fake logs)
        
        5.1.2 Deleting AWS CloudTrail logs
        
    5.2 AI-Assisted Evasion [AND]
    
        5.2.1 AI-generated fake traffic (mimicking normal behaviour)
        
        5.2.2 Deepfake video calls to bypass MFA (2023+)
```

## Key trends in IP attacks

* Cloud-native attacks: AWS/Azure/GCP exploitation
* AI-powered social engineering: deepfake audio/video
* Living-off-the-land (LOTL) techniques: abusing legitimate tools
* Supply chain compromises: software dependencies, SaaS vendors

## Defence trends 

* From Perimeter to Zero Trust: Assuming breach; verify every access request.
* AI & Automation: Real-time anomaly detection.
* Cloud-First Security: CSPM, CWPP, and CIEM (Cloud Infrastructure Entitlement Management).
* Resilience Over Prevention: Focus on rapid detection/response (for example MITRE Shield).
* Regulatory Pressure: Compliance with NIS2 (EU), SEC Cyber Rules (2023).

## Emerging tech

* Confidential Computing (for example Intel SGX, Azure Confidential VMs) to protect IP in use.
* Post-Quantum Cryptography Prep (NIST’s CRYSTALS-Kyber for future-proofing).


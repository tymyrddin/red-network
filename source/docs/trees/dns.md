# Domain Name System (DNS)

ometimes used for resolving BGP router IDs or peer names (though BGP primarily uses IP addresses).

## Compromise DNS Infrastructure or Data Exfiltration

```text
1. Exploit Protocol Weaknesses

    1.1 Cache Poisoning

        1.1.1 Exploit weak TXID entropy in DoH resolvers OR

        1.1.2 Side-channel attack on DoT implementations

        Prerequisite: AND (Attacker can intercept traffic AND resolver lacks DNSSEC)

    1.2 DDoS Amplification

        1.2.1 Abuse misconfigured DoQ resolvers OR

        1.2.2 Weaponize DNSSEC (NSEC3 walking)

        Prerequisite: AND (Open resolver available AND vulnerable payload size)

2. Attack Encrypted DNS

    2.1 Privacy Leaks

        2.1.1 Correlate DoH metadata (IP + timestamps) OR

        2.1.2 ML-based fingerprinting of encrypted traffic

    2.2 Downgrade Attacks

        2.2.1 Force fallback to plaintext DNS via TCP RST injection AND

        2.2.2 Disable ECH (Encrypted Client Hello) in DoH

3. Cloud/SaaS Exploits

    3.1 Kubernetes DNS Compromise

        3.1.1 Poison CoreDNS cache AND

        3.1.2 Bypass NetworkPolicy rules

    3.2 Serverless Abuse

        3.2.1 Lambda DNS tunneling (TXT exfiltration) OR

        3.2.2 Azure Private Resolver spoofing

4. Supply Chain Attacks

    4.1 Registrar Hijacking

        4.1.1 Steal API keys (Cloudflare, Route 53) OR

        4.1.2 Social engineer registrar support (post-GDPR WHOIS gaps)

    4.2 Subdomain Takeover

        4.2.1 Find dangling CNAME (GitHub Pages) AND

        4.2.2 Deploy malicious content

5. AI/ML-Augmented Attacks

    5.1 Evasion

        5.1.1 Poison DNS reputation models OR

        5.1.2 Generate benign-looking queries (mimic CDN traffic)

    5.2 Phishing Automation

        5.2.1 LLM-generated homograph domains AND

        5.2.2 Dynamic DNS fast-flux

6. Post-Quantum Threats

    6.1 Cryptographic Harvesting

        6.1.1 Collect ECDSA-P256 DNSSEC records AND

        6.1.2 Store for future quantum decryption

    6.2 QKD Spoofing

        6.2.1 Photon-splitting attack on QKD OR

        6.2.2 Fake QKD handshake
```

## Key trends in DNS attacks

* Encrypted DNS (DoH/DoT/DoQ) Exploitation: Attackers correlating metadata (IPs, timestamps) from DNS-over-HTTPS (DoH) or DNS-over-TLS (DoT) to track users despite encryption; Forcing victims to fall back to unencrypted DNS via TCP RST injection or QUIC handshake manipulation; Machine learning analyzes encrypted DNS patterns to infer visited domains.
* Cloud & Kubernetes-Targeted Attacks: Attackers abusing AWS Route 53, Azure DNS, or Cloudflare Gateway for DNS tunneling (exfiltration via TXT/AAAA records) or Phantom domain attacks (overloading resolvers with fake queries).
* Kubernetes DNS Threats: CoreDNS poisoning (malicious pods altering cluster resolution) and Sidecar proxy attacks (Istio’s DNS spoofing).
* AI-Driven & Automated Attacks: AI-Generated Phishing Domains and Adversarial ML: Poisoning DNS reputation models (tricking security tools into whitelisting malicious domains).
* Supply Chain & Registrar Compromises: 
    * Registrar Hacks: API key theft (GoDaddy, Namecheap breaches) leading to domain hijacking.
    * Subdomain Takeovers: Exploiting dangling CNAMEs in cloud services (AWS S3, GitHub Pages).
    * TLD-Level Attacks: Manipulating new gTLDs (.app, .xyz) via registrar collusion or ICANN policy gaps.
* Post-Quantum DNS Threats: Attackers collecting DNSSEC-signed records for future quantum-enabled breaks (ECDSA-P256) and Quantum key distribution (QKD) spoofing attacks targeting DNSSEC-authenticated zones.
* Advanced DDoS & Amplification: QUIC’s UDP-based protocol enables new amplification vectors; Exploiting NSEC3 walking or large RSA-4096 signatures to overwhelm resolvers.
* Zero-Trust & Internal DNS Abuse
    * ADIDNS Attacks: Manipulating Active Directory DNS for lateral movement.
    * Split-Horizon DNS Exploits: Bypassing internal DNS segmentation via DNS rebinding.
* Geopolitical & State-Sponsored DNS Attacks
    * DNSpionage: Nation-states hijack domains to redirect traffic (Sea Turtle campaign).
    * Internet Shutdowns: Governments manipulate DNS to censor domains (Russia’s Sovereign Runet).

## Defence trends

* Adoption of Encrypted DNS Protocols: DNS-over-HTTPS (DoH) / DNS-over-TLS (DoT) prevents eavesdropping and MITM attacks and major browsers (Chrome, Firefox) and OSes now default to DoH.
    * Oblivious DoH (ODoH) decouples client IP from queries via a proxy, enhancing privacy, used by Cloudflare, Apple, and Google.
* DNSSEC Deployment & Modernization
    * Wider Adoption: Governments (U.S. BOD 22-01) and enterprises enforce DNSSEC validation.
    * Post-Quantum Prep: Testing hybrid signatures (ECDSA + Falcon-1024) for future-proofing.
    * Automated Key Rollovers: Tools like OpenDNSSEC 2.2+ streamline key management.
* AI/ML-Powered Threat Detection: Anomaly Detection by ML models flag DNS tunneling (long subdomains, high TXT query volumes), and Darktrace, Cisco Umbrella, and F5 DNS Defense using behavioural AI.
* Generative AI for Threat Intel: Predicts typosquatting domains and fast-flux botnets via LLMs.
* Zero Trust & DNS Filtering: 
    * DNS-as-a-Security Layer: Solutions like Cloudflare Gateway and Zscaler DNS Security enforce policies at the DNS level.
    * RPZ (Response Policy Zones): Blocks malicious domains in real-time using threat feeds (Quad9, ThreatFox).
* Cloud-Native DNS Protections:
    * Kubernetes Hardening: CoreDNS plugins (k8s_gateway) prevent cluster-level poisoning; NetworkPolicy rules restrict DNS traffic between pods.
    * Serverless DNS Security: AWS GuardDuty monitors Route 53 for suspicious activity (domain hijacking).
* Anti-DDoS & Resilience Measures:
    * Anycast DNS: Cloud providers (AWS, Cloudflare) absorb DDoS attacks via global anycast networks.
    * Rate Limiting & Sinkholing: BIND 9.16+ and Knot DNS mitigate amplification attacks.
* Supply Chain & Registrar Defenses: 
    * Registry Lock: Requires manual approval for domain changes (Verisign’s Advanced Protection).
    * DMARC/DKIM/SPF: Combats DNS-based email spoofing (BEC, phishing).
    * DNSSEC for Email: MTA-STS and DANE (DNS-based Authentication of Named Entities) validate SMTP servers.
* Internal DNS Protections: 
    * ADIDNS Hardening: Microsoft’s Safe DNS Updates and EPA (Enhanced Protected Mode) for Active Directory.
    * DNS Sinkholing: Redirects malicious internal DNS queries to a logging server (Cisco Umbrella).
* Post-Quantum Readiness:
    * Experimentation with PQ Algorithms: Cloudflare and Google test CRYSTALS-Kyber for future DNSSEC.
    * Key Rotation Policies: Shorter key lifespans to reduce harvesting risks.
* Threat Intelligence Sharing:
    * Collaborative Platforms: MISP, FS-ISAC, and CISA’s AIS share DNS threat indicators.
    * Automated Blocklists: Integrations with Quad9, OpenPhish, and PhishTank.


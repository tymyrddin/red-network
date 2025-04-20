# MD5 (for TCP-AO)

Older BGP implementations used MD5 for securing BGP sessions (now deprecated in favor of stronger mechanisms).

## General MD5 attack tree

```text
1. Collision Attacks (OR)

    1.1 Prerequisites (AND):
    
        1.1.1 Ability to generate arbitrary MD5 inputs
        
        1.1.2 Target system accepts MD5 collisions (file verification, digital signatures)
        
    1.2 Attack Methods (OR):
    
        1.2.1 Chosen-Prefix Collision Attack (Marc Stevens, 2019)
        
            1.2.1.1 Allows two different inputs with the same MD5 hash while controlling large portions of both files
            
            1.2.1.2 Used in real-world attacks against certificate forgery and file spoofing
            
        1.2.2 Improved HashClash Techniques (2020+)
        
            1.2.2.1 Faster collision generation using optimized differential paths
            
            1.2.2.2 Can generate collisions in hours on modern GPUs

2. GPU/Cloud-Based Bruteforce (OR)

    2.1 Prerequisites (AND):
    
        2.1.1 Known hash format (password hashes without salt)
        
        2.1.2 Weak input space (common passwords, short strings)

    2.2 Attack Methods (OR):
    
        2.2.1 RTX 4090 Bruteforce (2023 Techniques)
        
            2.2.1.1 Achieves ~100 billion MD5 hashes/sec for simple passwords
            
            2.2.1.2 Can crack 8-character alphanumeric passwords in under a day
            
        2.2.2 AWS p4d.24xlarge Cluster Attack
        
            2.2.2.1 Uses NVIDIA A100 GPUs for distributed MD5 cracking
            
            2.2.2.2 Effective against unsalted password databases

3. Rainbow Table Adaptations (OR)

    3.1 Prerequisites (AND):
    
        3.1.1 No salt or known salt
        
        3.1.2 Target uses common input space (passwords, predictable strings)

    3.2 Attack Methods (OR):
    
        3.2.1 RainbowCrack Modern Tables (2022)
        
            3.2.1.1 Updated tables optimized for MD5 with common password patterns
            
            3.2.1.2 Hybrid approach combining dictionary and rainbow tables
            
        3.2.2 GPU-Optimized Rainbow Table Lookups
        
            3.2.2.1 Uses CUDA acceleration for faster lookups compared to traditional methods

4. Side-Channel Attacks (OR)

    4.1 Prerequisites (AND):
    
        4.1.1 Physical/cloud proximity to target
        
        4.1.2 Vulnerable implementation (software using MD5 insecurely)

    4.2 Attack Methods (OR):
    
        4.2.1 Cache Timing Attacks (MD5Leak, 2021)
        
            4.2.1.1 Recovers internal MD5 state by analyzing CPU cache access patterns
            
            4.2.1.2 30-40% faster than pre-2018 methods
        
        4.2.2 Power Analysis on IoT Devices
        
            4.2.2.1 Extracts MD5 hashes from embedded devices via power fluctuations

5. Protocol/Implementation Exploits (OR)

    5.1 Prerequisites (AND):
    
        5.1.1 System uses MD5 in a vulnerable way (legacy protocols)

    5.2 Attack Methods (OR):
    
        5.2.1 TLS 1.2 MD5 Certificate Forgery (2022 Research)
        
            5.2.1.1 Exploits servers still accepting MD5-based certificates
            
        5.2.2 Git Collision Attacks (2023 Demonstrations)
        
            5.2.2.1 Crafting two different Git objects with the same MD5 hash
            
        5.2.3 MD5-in-HMAC Exploitation (2021 Weaknesses)
        
            5.2.3.1 Some HMAC-MD5 implementations remain vulnerable to collision-based attacks
```

## Compromise BGP session via MD5 exploitation

```text
1. Goal: Compromise BGP Session via MD5 Exploitation (OR)

    1.1. Attack Path: MD5 Password Cracking (AND)
    
    Prerequisites:
    
        1.1.1. BGP session uses MD5 authentication (known or suspected)
        
        1.1.2. Attacker can capture BGP packets (MITM position, compromised router)
        
        1.1.3. No IPsec or additional encryption protecting BGP traffic
        
        Steps (OR):
        
        1.1.4. Extract MD5 hash from BGP packets (TCP Option 19)
        
        1.1.5. Perform offline cracking:
        
            1.1.5.1. GPU Bruteforce (AND)
            
                1.1.5.1.1. Use RTX 4090 (~100 GH/s) or cloud-based cracking
                
                1.1.5.1.2. Apply common BGP password patterns (router vendor defaults)
                
            1.1.5.2. Rainbow Table Attack (AND)
        
                1.1.5.2.1. Precomputed tables for known BGP MD5 passwords
                
                1.1.5.2.2. Requires unsalted MD5 (common in older BGP implementations)
    
    1.2. Attack Path: MD5 Collision-Based Session Hijacking (AND)
    
        Prerequisites:
        
        1.2.1. BGP peers accept MD5-based TCP sessions
        
        1.2.2. Attacker can inject packets into BGP session path
        
        Steps (OR):
        1.2.3. Chosen-Prefix Collision Attack (AND)
        
            1.2.3.1. Generate two different BGP OPEN messages with same MD5 hash
            
            1.2.3.2. Force session reset and impersonate legitimate peer
            
        1.2.4. HashClash-Style Session Injection (AND)
        
            1.2.4.1. Craft malicious BGP UPDATE with valid MD5 checksum
            
            1.2.4.2. Exploit routers that don’t validate BGP attributes post-MD5 check
    
    1.3. Attack Path: Side-Channel MD5 Key Extraction (AND)
    
        Prerequisites:
        
        1.3.1. Physical/network proximity to BGP router
        
        1.3.2. Router uses software-based MD5 (Linux/quagga implementations)
        
        Steps (OR):
        
        1.3.3. Cache Timing Attack (AND)
        
            1.3.3.1. Probe MD5 computation timing during BGP session establishment
            
            1.3.3.2. Recover secret key via statistical analysis
        
        1.3.4. Power Analysis (AND)
    
        1.3.4.1. Measure power fluctuations during MD5-HMAC computation (for devices with weak isolation)

2. Post-Compromise BGP Attacks (OR)

    2.1. Route Injection (AND)
    
        2.1.1. Advertise malicious routes after MD5 bypass
        
        2.1.2. Trigger route leaks or blackholes
    
    2.2. Persistent Session Takeover (AND)
    
        2.2.1. Maintain forged BGP session using cracked/stolen MD5 key
        
        2.2.2. Eavesdrop on all BGP updates
```

## Key trends

* Chosen-Prefix Collisions: Building on the 2017 shattered attack, researchers demonstrated practical attacks where attackers can craft two arbitrary files with the same MD5 hash while controlling large portions of both files.
* GPU Bruteforce: Modern GPUs can test billions of MD5 hashes per second (RTX 4090 achieves ~100 GH/s for simple passwords).
* Cloud-Based Attacks: AWS p4d instances with 8xA100 GPUs can crack most unsalted MD5 hashes of passwords <12 chars in hours.
* Side-Channel: New timing attacks can recover MD5 states 30-40% faster than pre-2018 methods.
* Protocol Exploits: Several implementations still use MD5 in legacy modes (especially in embedded systems) where modern attacks apply.

## Key Defence Trends Against MD5 Exploitation in BGP

* Cryptographic Replacement: 
    * Adoption of TCP-AO replaces MD5 with AES-based CMAC gives stronger integrity and per-packet key derivation to prevent replay attacks
    * IPsec Encapsulation for BGP encrypts all BGP traffic (IKEv2 + ESP). It is mandated in high-security environments (financial/military ASes)
* Operational Hardening: 
    * Strict Key Management: Regular rotation of BGP session keys (even if MD5 is used) and Hardware Security Modules (HSMs) for key storage
    * Network Segmentation: BGP speakers placed in isolated management VLANs; Physical access controls for core routers
* Detection & Mitigation:
    * Anomaly Detection Systems: ML-based monitoring of BGP updates (RPKI invalid announcements); Threshold alerts for sudden AS path changes
    * Forensic Readiness: PCAP logging of BGP sessions (post-mortem collision analysis); MD5 hash blacklisting for known malicious prefixes
* Legacy MD5 Mitigations:
    * Salting Where Unavoidable: Router-specific salts for MD5 keys (breaks rainbow tables)
    * Rate-Limited Sessions: Lockout after repeated failed MD5 auth attempts; TCP RST injection for suspicious sessions
* Vendor/Protocol Trends:
    * BGP Software Updates: Disabling MD5 by default (FRRouting 8.0+); Deprecation warnings in Cisco IOS-XE/Junos
    * RPKI Adoption: ROA-based route origin validation (reduces impact of hijacking); ASPA for path validation (draft-ietf-sidrops-aspa-10)

## Emerging Defences

* Post-Quantum Signatures: Testing CRYSTALS-Dilithium for BGPsec
* AI-Powered BGP Defences: Real-time collision detection via neural nets
* Hardware Enforced [TCP-AO](tcp-ao.md): Offload to NICs/DPUs for performance


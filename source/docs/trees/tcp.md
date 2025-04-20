# Transmission Control Protocol (TCP)

## Compromise a target via TCP vulnerabilities

```text
1. Exploit TCP Stack Vulnerabilities (OR)

    1.1. Remote Code Execution (RCE) via TCP/IP flaws
    
    1.2. Kernel memory corruption (SACK-based attacks)

2. Protocol-Level Attacks (OR)

    2.1. Connection Hijacking (AND)
    
        2.1.1. Off-path sequence number prediction
        
        2.1.2. Malicious packet injection (RST/FIN spoofing)
    
    2.2. Amplification/Reflection Attacks (OR)
    
        2.2.1. TCP middlebox reflection
        
        2.2.2. ACK/PSH flood abuse

3. Off-Path & Side-Channel Attacks (AND)

    3.1. Blind In-Window Exploit (OR)
    
        3.1.1. NAT Slipstreaming variants
        
        3.1.2. Protocol downgrade attacks (QUIC-to-TCP)
    
    3.2. Side-Channel Data Extraction (AND)
    
        3.2.1. TCP timestamp analysis
        
        3.2.2. Application data correlation

4. Cloud/Middlebox-Specific Attacks (OR)

    4.1. Bypass Cloud Load Balancers (AND)
    
        4.1.1. Crafted TCP segmentation evasion
        
        4.1.2. Instance resource exhaustion
    
    4.2. Stateful Firewall Evasion (OR)
    
        4.2.1. TCP Fast Open (TFO) cache poisoning
        
        4.2.2. Fragmentation overlap attacks

5. AI/ML-Enhanced Attacks (AND)

    5.1. Traffic Fingerprinting (OR)
    
        5.1.1. Encrypted traffic classification
        
        5.1.2. SCADA system detection via flow patterns
    
    5.2. Adversarial Traffic Generation (AND)
    
        5.2.1. GAN-based normal traffic modeling
        
        5.2.2. Stealthy DDoS payload synthesis
```

## Manipulate BGP routing by compromising TCP-based BGP sessions

BGP uses TCP (Transmission Control Protocol) as its transport layer protocol. TCP establishes a reliable, 
connection-oriented session between BGP peers (routers). BGP peers communicate default over TCP port 179.

```text
1. Disrupt BGP Session Establishment (OR)

    1.1. TCP SYN Flood Attack (Exhaust BGP Peer Resources)
    
    1.2. Exploit BGP’s MD5 Authentication Weaknesses (OR)
    
        1.2.1. Crack TCP-MD5 Hashes (if weak keys used)
        
        1.2.2. Bypass MD5 via TCP Session Hijacking

2. Hijack Active BGP Sessions (AND)

    2.1. Predict BGP TCP Sequence Numbers (OR)
    
        2.1.1. Off-Path ISN Prediction (using timestamp leaks)
        
        2.1.2. In-Window Guessing (due to poor ISN randomization)
    
    2.2. Inject Malicious BGP Updates (OR)
    
        2.2.1. Spoofed Route Advertisements
        
        2.2.2. Crafted AS_PATH Manipulation

3. Exploit TCP Stack Vulnerabilities on BGP Routers (OR)

    3.1. Trigger Kernel Crashes (DoS) (OR)
    
        3.1.1. Exploit TCP SACK Handling (Linux CVE-2019-11477)
        
        3.1.2. Abuse TCP Selective ACK (SACK) Resource Exhaustion
    
    3.2. Remote Code Execution (RCE) via TCP/IP Stack (AND)
    
        3.2.1. Exploit Router OS TCP Stack (JunOS, IOS XR flaws)
        
        3.2.2. Deploy Malicious BGP Configurations Post-Exploit

4. Man-in-the-Middle (MITM) BGP Sessions (AND)

    4.1. Intercept TCP Traffic (OR)
    
        4.1.1. ARP/DNS Spoofing to Redirect BGP Traffic
        
        4.1.2. BGP Peering Over Unencrypted Links (Internet Exchange Points)
    
    4.2. Decrypt or Modify BGP Messages (OR)
    
        4.2.1. Downgrade TCP-MD5 to Plaintext (if misconfigured)
        
        4.2.2. Exploit Missing TCP-AO (Authentication Option)

5. Abuse BGP Session Persistence (OR)

    5.1. Force BGP Session Resets via TCP Attacks (AND)
    
        5.1.1. Inject RST Packets (Precision Spoofing)
        
        5.1.2. Exploit TCP Keepalive Timeouts
    
    5.2. Subvert BGP Graceful Restart (OR)
    
        5.2.1. Spoof Graceful Restart Capabilities
        
        5.2.2. Exhaust Router Memory During Recovery
```

## Key trends in TCP attacks

* Shift to Off-Path Exploits: Bypassing traditional encryption (for example, QUIC-to-TCP leaks).
* Kernel & Middlebox Focus: Cloud and firewall evasion techniques.
* AI/ML in Attacks: Adversarial learning to evade detection.

## Defence trends 

* Shift to Memory-Safe Stacks: Reducing RCE risks (for example, Rust in Linux networking).
* Encryption Everywhere: QUIC and TCP-AO to prevent injection/hijacking.
* AI vs. AI Arms Race: Defenders use ML to detect adversarial TCP flows.
* Cloud-Native Protections: eBPF and Kubernetes policies for granular control.


# Internet Control Message Protocol (ICMP)

ICMP is used for error reporting (for example, if a BGP peer is unreachable).

## Exploit ICMP for malicious purposes

```text
1. Reconnaissance & Network Mapping

    1.1 ICMP Echo Sweeping (Ping Sweep)

        OR: Use fping for parallel scans

        OR: Custom low-rate ICMP probes to evade detection

    1.2 TTL Manipulation for OS Fingerprinting

        AND: Send ICMP Echo with varying TTLs

        AND: Analyze TTL decay patterns

    1.3 ICMP-based Service Discovery

        OR: Abuse ICMP Timestamp Requests

        OR: Leverage ICMP Address Mask Requests

2. Data Exfiltration & Covert Channels

    2.1 ICMP Tunneling

        AND: Encode data in ICMP Echo payloads

        AND: Use tools like icmptunnel (IPv6-enabled)

    2.2 Fragmented ICMP Exfiltration

        OR: Exploit IPv6 fragmentation for DPI evasion

        OR: Split payloads across ICMP packets

    2.3 DNS-over-ICMP (C2)

        AND: Encode DNS queries in ICMP Echo

        AND: Use malware like MosaicLoader for callbacks

3. Denial-of-Service (DoS) & Amplification

    3.1 ICMP Floods

        OR: Direct IPv6 ping6 floods

        OR: Spoofed-source ICMPv6 floods

    3.2 ICMP Amplification

        AND: Spoof victim IP in "Packet Too Big" messages

        AND: Reflect traffic via misconfigured cloud hosts

    3.3 Ping of Death (Modern Variants)

        OR: IPv6 jumbo frames targeting IoT kernels

        OR: Malformed ICMPv6 packets crashing routers

4. Evasion & Protocol Abuse

    4.1 NAT/Firewall Bypass

        AND: Use ICMP Echo Replies for C2 callbacks

        AND: Abuse whitelisted ICMP types (PMTUD)

    4.2 Lateral Movement via ICMP

        OR: APT29-style internal C2 channels

        OR: ICMP-based password spraying (APT41)

    4.3 ICMPv6 Router Advertisement Spoofing

        AND: Send rogue RAs to hijack traffic

        AND: Exploit weak IPv6 neighbor discovery

5. Zero-Day & Hardware Exploits

    5.1 ICMP Side-Channel Attacks

        OR: NetSpectre-style timing leaks

        OR: Infer VM placement via ICMP TTL (cloud)

    5.2 IoT/OT Device Crashes

        AND: Send malformed ICMPv6 to embedded devices

        AND: Trigger firmware bugs (CVE-2020-10148)

    5.3 Cloud Metadata Service Abuse

        OR: ICMP-based IMDSv1 queries (AWS)

        OR: ICMP-triggered SSRF in serverless apps
```

## Key trends in ICMP attacks

* IPv6 Abuse: ICMPv6 is heavily exploited due to poor default filtering.
* Cloud Targeting: Attacks leverage ICMP for cloud metadata API abuse.
* IoT/OT Focus: Fragmentation and malformed ICMP break embedded stacks.
* Stealth: Low-and-slow ICMP tunnels evade traditional SIEMs.

## Defence trends

* Network-Level Defenses: Strict ICMP Rate Limiting; IPv6 ICMPv6 Hardening; Fragmentation Control
* Zero Trust & Segmentation: Microsegmentation; Egress Filtering
* Deep Packet Inspection (DPI) & IDS/IPS: Payload Analysis; Behavioural Detection
* Cloud-Specific Protections: Metadata Service Lockdown; Cloud-Native Firewalls
* Endpoint & IoT Protections: Host-Based ICMP Stack Hardening; Kernel Protections
* Threat Intelligence & Automation: Threat Feeds; SOAR Playbooks

## Emerging defence trends

* ML-Based Traffic Profiling: Detecting ICMP tunnels via entropy analysis of payloads (for example Palo Alto ML-Powered NGFW).
* QUIC/HTTP/3 Monitoring: ICMP used for QUIC path validation—filter malicious probes.
* Hardware-Assisted Filtering: SmartNICs offloading ICMP flood mitigation (for example AWS Nitro Cards).


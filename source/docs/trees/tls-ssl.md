# TLS/SSL (for BGPsec)

In secure implementations like BGPsec, cryptographic protections (for example RPKI) may use TLS-like mechanisms to verify 
route authenticity.

## Compromise TLS/SSL Security

(OR: Any branch succeeds)

```text
1. Cryptographic Attacks

    Prerequisite: Support for weak algorithms/protocols 
    (OR)

    1.1 Key Exchange Compromise
    (AND)

        1.1.1 Use of RSA/DH with <2048 bits

        1.1.2 No PFS (Perfect Forward Secrecy)

        Example: Logjam (DH), ROBOT (RSA)

    1.2 Cipher Suite Exploitation
    (OR)

        1.2.1 CBC padding oracle (Lucky13 variants)

        1.2.2 64-bit block cipher (Sweet32)

        Prerequisite: Legacy cipher support (AES-CBC, 3DES)

2. Protocol Exploits

    Prerequisite: Misconfigured TLS stack (OR)

    2.1 Handshake Manipulation
    (OR)

        2.1.1 TLS 1.3 downgrade to 1.2
        (AND)

            Middlebox interference

            Client accepts fallback

        2.1.2 0-RTT replay attacks
        (AND)

            TLS 1.3 early data enabled

            No replay protections

    2.2 Cross-Protocol Attacks 
    (AND)

        2.2.1 Shared ports (e.g., HTTPS/SMTP)

        2.2.2 Weak ALPN validation

        Example: ALPACA (2021)

3. Implementation Flaws

    Prerequisite: Unpatched libraries
    (OR)

    3.1 Memory Corruption
    (AND)

        3.1.1 Vulnerable OpenSSL (e.g., CVE-2022-3602)

        3.1.2 Malicious packet injection

    3.2 Side-Channel Leaks
    (OR)

        3.2.1 Timing attacks (Minerva)

        3.2.2 Power analysis (ROBOT)

        Prerequisite: Non-constant-time implementations

4. PKI Attacks

    Prerequisite: Weak certificate validation
    (OR)

    4.1 CA Compromise
    (OR)

        4.1.1 CA misissuance (e.g., Let's Encrypt CAA bypass)

        4.1.2 Trust in legacy root CAs

    4.2 Revocation Bypass
    (AND)

        4.2.1 OCSP/CRL not enforced

        4.2.2 Stapling not required

5. Post-Compromise Attacks

    Prerequisite: Session key exposure
    (AND)

    5.1 Log encrypted traffic

    5.2 Break encryption later
    (OR)

        5.2.1 Quantum computing (store now/decrypt later)

        5.2.2 Weak key generation
```

## Compromise BGP via TLS/SSL Weaknesses

(OR: Any branch below achieves the root goal)

```text
1. Exploit Weak TLS Handshake in BGP Sessions

    Prerequisite: BGP routers use outdated TLS (e.g., OpenSSL 1.1.1 or older).
    (OR: Either sub-path works)

    1.1. Downgrade BGP-over-TLS to Legacy Protocols
    (AND: Requires all conditions)

        1.1.1. Attacker controls MITM position (e.g., ISP/IXP)

        1.1.2. Router supports TLS 1.2 or lower (prerequisite: outdated TLS)

        1.1.3. No TLS downgrade protection (e.g., missing HSTS for BGP APIs)

    1.2. Force Weak Cipher Suites
    (AND: Requires all conditions)

        1.2.1. Router accepts deprecated ciphers (e.g., AES-CBC-SHA) (prerequisite: outdated TLS)

        1.2.2. Attacker modifies ClientHello to exclude strong ciphers

        1.2.3. No cipher suite pinning enforced

2. Bypass Certificate Validation

    Prerequisite: Weak CA trust anchors (e.g., accepting legacy CAs).
    (OR: Either sub-path works)

    2.1. Obtain Fraudulent BGP/TLS Certificate
    (AND: Requires all conditions)

        2.1.1. Exploit CA misissuance (e.g., Let’s Encrypt CAA bypass) (prerequisite: weak CA trust)

        2.1.2. Validate ownership via BGP hijack (e.g., fake ROA) (prerequisite: missing RPKI/ROV)

        2.1.3. CA does not enforce IP ownership cross-checks

    2.2. Disable Revocation Checks
    (AND: Requires all conditions)

        2.2.1. Block OCSP/CRL requests (via DNS/BGP hijack)

        2.2.2. Router ignores revocation status (e.g., stale CRL) (prerequisite: weak CA trust)

        2.2.3. No OCSP stapling enforced

3. Exploit Implementation Vulnerabilities

    Prerequisite: BGP routers use outdated TLS libraries.
    (OR: Either sub-path works)

    3.1. Memory Corruption in TLS Stack
    (AND: Requires all conditions)

        3.1.1. Router uses vulnerable OpenSSL (e.g., CVE-2022-3602) (prerequisite: outdated TLS)

        3.1.2. Attacker sends malformed packets (e.g., crafted ClientHello)

        3.1.3. No exploit mitigations (e.g., ASLR, stack canaries)

    3.2. Side-Channel Attack on BGP Router
    (AND: Requires all conditions)

        3.2.1. Router leaks timing info (e.g., Minerva ECDSA flaw) (prerequisite: outdated TLS)

        3.2.2. Attacker measures handshake response times

        3.2.3. No constant-time crypto implemented

4. Attack BGP Management Plane (TLS-Enabled APIs)

    Prerequisite: Weak CA trust or misconfigured admin interfaces.
    (OR: Either sub-path works)

    4.1. Spoof BGP Configuration API
    (AND: Requires all conditions)

        4.1.1. Obtain rogue cert for bgp-manage.example.com (prerequisite: weak CA trust)

        4.1.2. Router trusts public CAs for API authentication

        4.1.3. No certificate pinning enforced

    4.2. Exploit Web-Based BGP Tools
    (AND: Requires all conditions)

        4.2.1. XSS in TLS-protected admin interface (prerequisite: misconfigured UI)

        4.2.2. Admin user clicks malicious link

        4.2.3. No CSP headers or input sanitization
```

## Compromise TLS/SSL via BGPsec Weaknesses

(OR: Any branch below achieves the root goal)

```text
1. Exploit BGPsec-Validated Route Hijacking

Prerequisite: Partial RPKI/BGPsec adoption (<100% deployment)
(OR: Choose one sub-path)

    1.1 BGPsec Key Compromise
    (AND: All required)

        1.1.1 Attacker steals BGPsec router private key (e.g., via supply chain)

        1.1.2 No HSM protection for keys

        1.1.3 Weak key rotation policies

    1.2 RPKI Misconfiguration Exploit
    (AND: All required)

        1.2.1 ROA overlaps in RPKI database

        1.2.2 Victim AS doesn't monitor route origins

        1.2.3 Attacker can announce hijacked prefix

2. TLS Certificate Spoofing via BGPsec

Prerequisite: CAs don't strictly validate IP ownership
(OR: Choose one sub-path)

    2.1 BGP-Hijacked IP Validation
    (AND: All required)

        2.1.1 BGPsec-validated route hijack (from Branch 1)

        2.1.2 CA accepts BGP-routed IPs for validation

        2.1.3 No secondary ownership checks (e.g., WHOIS)

    2.2 RPKI-TLS Trust Collision
    (AND: All required)

        2.2.1 Malicious ROA for shared IP space

        2.2.2 CA issues cert based on RPKI alone

        2.2.3 No certificate transparency monitoring

3. BGPsec-Enabled MITM Attacks

Prerequisite: Networks trust BGPsec-validated routes blindly
(OR: Choose one sub-path)

    3.1 Route Injection + TLS Strip
    (AND: All required)

        3.1.1 BGPsec-validated malicious route

        3.1.2 Victim accepts routes without additional checks

        3.1.3 Middlebox strips TLS 1.3 to force HTTP

    3.2 QUIC Redirection Attack
    (AND: All required)

        3.2.1 BGPsec hijack of QUIC endpoint IPs

        3.2.2 No QUIC connection migration validation

        3.2.3 0-RTT enabled (allows replay)
```


## Notes


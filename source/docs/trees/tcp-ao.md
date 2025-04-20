# TCP-AO (Authentication Option)

## Bypass or compromise TCP-AO protection

```text
1. Bypass or compromise TCP-AO protection (OR)

    1.1 Attack Path: Key Compromise (AND)
    
    Prerequisites:
    
        1.1.1 Physical or logical access to key storage
        
        1.1.2 Weak key generation/management
    
    Methods (OR):
    
        1.1.3 Brute Force Weak Keys (AND)
        
            1.1.3.1 Exploit short/insecure Master Key Tuple (MKT)
            
            1.1.3.2 Use GPU/cloud cracking (if key entropy < 128 bits)
            
        1.1.4 Side-Channel Key Extraction (AND)
        
            1.1.4.1 Cache timing attack on AES-CMAC computation
            
            1.1.4.2 Power analysis on hardware modules (HSMs, TPMs)
        
    1.2 Attack Path: Cryptographic Weakness Exploitation (AND)
    
    Prerequisites:
    
        1.2.1 Implementation uses vulnerable AES-CMAC mode
        
        1.2.2 Attacker can observe/modify TCP-AO packets

    Methods (OR):
    
        1.2.3 Forged MAC with Nonce Reuse (AND)
        
            1.2.3.1 Exploit improper ISN (Initial Sequence Number) handling
            
            1.2.3.2 Replay packets with identical (KeyID, ISN) pairs
            
        1.2.4 Algorithm Downgrade (AND)
        
            1.2.4.1 Force fallback to MD5 via TCP injection (if legacy support enabled)
            
            1.2.4.2 Exploit misconfigured "accept-ao-mismatch" settings

    1.3 Attack Path: Session Hijacking (AND)
    
    Prerequisites:
    
        1.3.1 Predictable ISN (weak RNG in endpoint)
        
        1.3.2 Ability to MITM TCP traffic
    
    Methods (OR):
    
        1.3.3 ISN Guessing + AO Bypass (AND)
        
            1.3.3.1 Predict ISN to spoof valid AO segments
            
            1.3.3.2 Exploit systems skipping AO checks on RST packets
            
        1.3.4 TCP-AO Session Resynchronization Attack (AND)
        
            1.3.4.1 Force resync via crafted SACK/retransmission
            
            1.3.4.2 Inject malicious data during rekeying window

2. Post-Compromise Attacks (OR)

    2.1 BGP Route Injection (AND)
    
        2.1.1 Advertise malicious routes via compromised BGP-over-TCP-AO session
        
        2.1.2 Suppress route withdrawals to create blackholes
    
    2.2 Persistent Eavesdropping (AND)
    
        2.2.1 Decrypt future sessions using stolen keys
        
        2.2.2 Modify TCP streams via AO-aware packet manipulation
```

## Emerging threats

* Quantum pre-computation attacks on static MKTs
* AI-assisted ISN prediction for hijacking
* Firmware exploits targeting NIC offload of AES-CMAC
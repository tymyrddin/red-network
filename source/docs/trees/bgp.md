# Border Gateway Protocol (BGP)

BGP is an application layer protocol (OSI Layer 7) that defines how routers exchange routing information. It uses 
path-vector routing (as opposed to distance-vector or link-state).

## Disrupt or Manipulate BGP Routing

```text
1. Prefix Hijacking

    OR Gate (Choose one method):

        1.1 Sub-Prefix Hijacking (More specific route)

        1.2 Exact-Prefix Hijacking (Same route, spoofed AS)

        1.3 ROA Bypass Attack (Exploit RPKI misconfigurations)

2. AS-Path Manipulation

    OR Gate:

        2.1 AS-Path Prepending (Fake path inflation)

        2.2 Ghost AS Insertion (Hide malicious AS)

        2.3 AI-Generated Path Spoofing (Evade heuristics)

3. Denial-of-Service (BGP Session Attacks)

    OR Gate:

        3.1 TCP RST Injection (Kill BGP sessions)

            AND Conditions:

                No TCP-AO/MD5

                Attacker on-path

        3.2 Route Flap DDoS (Flood updates)

4. Traffic Interception (Espionage)

    AND Gate (Requires multiple steps):

        4.1 Prefix Hijacking (OR from Section 1)

        4.2 AS-Path Manipulation (OR from Section 2)

        4.3 Decryption/Passive Snooping (MitM position)

5. Exploiting RPKI Weaknesses

    OR Gate:

        5.1 Stale ROA Attack (Use expired ROAs)

        5.2 Fraudulent ROA Registration (Social engineering RIRs)

6. Cross-Protocol Attacks

    AND Gate (BGP + Another vulnerability):

        6.1 BGP + DNS Hijacking

            OR Sub-options:

                Redirect DNS resolvers

                Poison DNS cache via fake routes

        6.2 BGP + CDN Manipulation

            Force traffic through malicious edge nodes

7. AI/ML-Assisted Attacks

    OR Gate:

        7.1 Automated ROA Gap Scanning

        7.2 ML-Generated AS-Path Spoofing

8. Supply Chain Compromise

    AND Gate (Requires access + exploitation):

        8.1 Compromise ISP/IXP (OR: Hack, Insider Threat)

        8.2 Propagate Malicious Routes
```

## Key trends in IP attacks

* Prefix Hijacking is evolving beyond RPKI: Attackers exploit RPKI adoption gaps, "ROA squatting," and partial deployment.
* AS-Path Manipulation uses more sophisticated evasion: Attackers mimic legitimate AS paths or use "AS hijacking" (faking AS ownership).
* BGP Denial-of-Service (DoS) Attacks: There seems to be a rise in state-sponsored "link-cutting" to isolate regions.
* Espionage & Surveillance via BGP: Nation-states hijack routes to intercept traffic (not just disrupt).
* Exploiting RPKI Weaknesses: Attackers target RPKI’s slow adoption and human errors.
* Cross-Protocol Attacks (BGP + Other Layers) BGP increasingly used to amplify other attacks (DNS, QUIC, etc.).
* AI/ML-Assisted BGP Attacks: AI automates reconnaissance and evasion.
* Supply Chain Compromise: Targeting Tier-2 ISPs and IXPs for broader impact.

## Defence trends

* RPKI + MANRS Adoption: Slow but growing (~40% RPKI coverage).
* AI-Powered BGP Monitoring: Tools like ARTEMIS use ML to detect anomalies.
* BGPsec Experiments: Limited deployment due to complexity.
* Geopolitical Filtering: ISPs drop routes from "untrusted" ASes.



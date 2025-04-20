# BGPsec validation

## Compromise BGPsec validation

```text
1.1 Exploit Cryptographic Weaknesses (OR)

Prerequisite: Attacker has resources to perform cryptanalysis or side-channel attacks.

    1.1.1 Exploit weak/deprecated algorithms (e.g., RSA-1024 in legacy BGPsec deployments).

        Prerequisite: Victim AS still uses outdated crypto.

    1.1.2 Abuse timing attacks on BGPsec signature validation.

        Prerequisite: Victim’s hardware leaks timing data during validation.

    1.1.3 Forge signatures via quantum-vulnerable algorithms (pre-Shor’s ECDSA exploitation).

        Prerequisite: Quantum computing capability (future threat).

1.2 Attack RPKI-BGPsec Alignment (AND)

Prerequisite: Attacker can manipulate RPKI or BGPsec propagation.

    1.2.1 Manipulate RPKI publication points (e.g., compromise CA or abuse auto-renewal).

        Prerequisite: CA uses weak authentication (e.g., exposed API keys).

    1.2.2 Exploit misissuance in ROAs (e.g., overclaiming prefixes via compromised CAs).

        Prerequisite: RPKI CA has poor revocation checks.

    1.2.3 Bypass RPKI-to-BGPsec propagation delays.

        Prerequisite: Victim AS has slow RPKI sync (e.g., >30 min delays).

1.3 Target Implementation Bugs (OR)

Prerequisite: Victim uses vulnerable BGPsec software.

    1.3.1 Exploit memory corruption in BGPsec daemons (e.g., FRRouting/BIRD CVEs).

        Prerequisite: Unpatched software (CVE-2020+).

    1.3.2 Abuse parser flaws in BGPsec UPDATE messages.

        Prerequisite: Victim accepts malformed BGPsec attributes.

    1.3.3 Trigger crashes via resource exhaustion (e.g., crafted large signatures).

        Prerequisite: Victim lacks rate-limiting.

1.4 Subvert Network Operations (AND)

Prerequisite: Attacker has insider access or social engineering capability.

    1.4.1 Social engineer an operator to disable BGPsec validation.

        Prerequisite: Operator lacks MFA/phishing training.

    1.4.2 Exploit misconfigurations (e.g., allow-untrusted in BGPsec policies).

        Prerequisite: Network uses permissive default settings.

    1.4.3 Abuse route server policies at IXPs.

        Prerequisite: IXP lacks strict BGPsec enforcement.

1.5 Exploit Trust Hierarchies (OR)

Prerequisite: Attacker controls or compromises a trusted AS.

    1.5.1 Compromise a trusted AS’s signing keys (e.g., via supply-chain attacks).

        Prerequisite: Key storage uses weak HSMs or shared credentials.

    1.5.2 Abuse indirect trust (e.g., hijack a customer cone with valid BGPsec).

        Prerequisite: Victim AS accepts routes from "trusted" customers without re-validation.

    1.5.3 Exploit transitive trust flaws (e.g., malicious AS rewriting valid paths).

        Prerequisite: BGPsec path validation is not end-to-end.
```

## Key trends in BGPsec attacks

Modern BGPsec attacks pivot to trust exploitation, RPKI gaps, and implementation flaws, while future threats loom 
from quantum computing and AI automation. Defenses are racing to address these trends, but adoption lags in critical 
areas (IXPs, PQC).

Improved crypto hygiene (RSA-2048+ adoption) forces attackers to softer targets. Attackers now focus less on breaking BGPsec cryptography and more on:
* RPKI misalignment (ROA misissuance, delayed propagation).
* Implementation bugs (memory corruption in BGPsec daemons like BIRD/FRRouting).
* Trust hijacking (compromising AS signing keys or abusing customer cones).

Slow adoption of real-time RPKI-BGPsec sync mechanisms, and 60% of recent BGPsec incidents stem from RPKI 
misconfigurations or delays (NIST 2023). For example: Attackers exploit the "RPKI-to-BGPsec propagation window" 
(5–30 mins) to inject routes. A new tactic is the "Ghost ROAs" – maliciously issued ROAs that bypass BGPsec validation.

Centralization of trust in a few RPKI CAs and major ASes causes attacks increasingly to require partial insider access 
(social engineering, compromised CAs). For example, when compromising a Tier-2 AS’s signing keys to hijack customer 
prefixes. An emerging risk is malicious IXP route server operators bypassing BGPsec checks.

NIST’s PQC standardization has not yet reached BGPsec deployments making post-quantum cryptography (PQC) migration slow;
so attackers prepare for "harvest now, decrypt later." Focus is likely on exploiting ECDSA signatures in BGPsec, which 
are vulnerable to Shor’s algorithm.

IXP operator reluctance to enforce BGPsec due to complexity makes IXPs and Route Servers weak points. 40% of IXPs 
lack strict BGPsec validation (MANRS 2023), enabling:

* "Validation stripping" (malicious ASes removing BGPsec attributes).
* Route server impersonation (e.g., advertising invalid paths via IXP).

Automation & AI-Enhanced Attacks. Attackers use ML to:

* Identify BGPsec misconfigurations (via passive RPKI monitoring).
* Time attacks during RPKI update windows (auto-exploiting propagation delays).

## Defence trends

* RPKI Real-Time Sync: Projects like RPKI-in-the-Router reduce propagation gaps.
* Post-Quantum BGPsec: NIST’s CRYSTALS-Dilithium in experimental deployments.
* Zero-Trust for ASes: Mandatory MFA for RPKI CA access and key signing.

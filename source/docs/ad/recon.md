# Recon active directory

## Attack tree

```text
1 Scan the network
2 No credentials/sessions
    2.2 Enumerate DNS (for example, with gobuster)
    2.3 Enumerate LDAP
    2.4 Poison the network
        1.4.1 Responder
        1.4.2 Relay attack
        1.4.3 Evil-SSDP
    2.5 OSINT
3 Valid username but no passwords
    3.1 ASREPRoast
    3.2 Password spraying
4 With credentials/sessions
    4.1 CMD
    4.2 powershell
    4.3 powerview
    4.4 Bloodhound
```
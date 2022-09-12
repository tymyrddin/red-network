# FTP exploits

## Attack tree

```text
1 Detect the presence of anonymous FTP
2 Detect version
3 Identify possible exploits
4 Exploit
```

## Notes

The File Transfer Protocol (FTP) is a service that allows the uploading and downloading of files from a machine 
running an FTP service. If the system is not patched, it may be vulnerable to an FTP exploit. FTP uses
TCP port 20 and 21.

Many applications make use of FTP for transferring files to and from hosts over the network. The FTP server provides 
read/write access to files and directories for remote users and inherited authentication and file sharing permissions 
from the local operating system.

Many types of vulnerabilities plague FTP applications: backdoors, directory traversals, DoS attacks, buffer overflow attacks, local and remote file inclu-
sion (LFI/RFI) attacks, and authentication bypass attacks, to name a few.

## Tools

* [Nmap ftp-anon.nse](https://nmap.org/nsedoc/scripts/ftp-anon.html)
* [Metasploit auxiliary module ftp_login](https://www.infosecmatter.com/metasploit-module-library/?mm=auxiliary/scanner/ftp/ftp_login)
* [Nmap ftp-syst.nse](https://nmap.org/nsedoc/scripts/ftp-syst.html)
* [Exploit-DB](https://www.exploit-db.com)
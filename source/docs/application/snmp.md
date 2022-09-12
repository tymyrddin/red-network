# SNMP exploits

## Attack tree

```text
1 Sniff cleartext SNMP communications 
2 Pose as an SNMP manager
3 Passively or actively inject XSS data or other improperly formatted strings
```

## Example

```text
msf > search snmp
msf > search scanner name:snmp
msf > search exploit name:snmp -S great
```
## Notes

The Simple Network Management Protocol (SNMP) is a protocol used to monitor and manage network devices. 
SNMP uses UDP port 161.

There are three basic categories of SNMP exploits:

* Sniffing cleartext SNMP communications between managers and agents to obtain the community string or information 
from the devices. This can include statistics about hardware, interface traffic, services, users, groups, route 
tables, listening ports, running processes, and much more.
* Posing as an SNMP manager, providing the correct community string, and enumerate information from SNMP agents.
* Exploiting the implicit trust SNMP managers have with the assets they manage. Most NMSs do not carefully validate 
the input from their agents. An adversary could passively or actively inject XSS data or other improperly formatted 
strings from the agent to the NMS. These could result in buffer overflows or arbitrary command injection.

## Tools

* [Nmap: snmp-brute.nse](https://nmap.org/nsedoc/scripts/snmp-brute.html)
* [Nmap: snmp-win32-software.nse](https://nmap.org/nsedoc/scripts/snmp-win32-software.html)
* [Nmap: snmp-win32-services.nse](https://nmap.org/nsedoc/scripts/snmp-win32-services.html)
* [Metasploit: auxiliary/scanner/snmp/snmp_enum](https://www.infosecmatter.com/metasploit-module-library/?mm=auxiliary/scanner/snmp/snmp_enum)
* [Metasploit: auxiliary/scanner/snmp/snmp_enumshares](https://www.infosecmatter.com/metasploit-module-library/?mm=auxiliary/scanner/snmp/snmp_enumshares)
* [Metasploit: auxiliary/scanner/snmp/snmp_enumusers](https://www.infosecmatter.com/metasploit-module-library/?mm=auxiliary/scanner/snmp/snmp_enumusers)
* [Metasploit: auxiliary/scanner/snmp/snmp_login](https://www.infosecmatter.com/metasploit-module-library/?mm=auxiliary/scanner/snmp/snmp_login)
* [Metasploit: exploit/multi/http/hp_sys_mgmt_exec](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/multi/http/hp_sys_mgmt_exec)
* [Metasploit: exploit/windows/http/h_nnm_snmp](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/windows/http/h_nnm_snmp)

## Resources

* [Managed to Mangled: SNMP Exploits for Network Management Systems](https://information.rapid7.com/managed-to-mangled-snmp-exploits-for-network-management-systems.html)
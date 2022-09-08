# Attacking LLMNR and NetBIOS

## Attack tree

```text
1 LLMNR/NBT-NS poisoning
    1.1 through SMB
    1.2 through WPAD
```

## Examples

### LLMNR/NBT-NS poisoning through SMB

1. Start responder with the NIC (for example `eth0`) to listen for LLMNR requests on. The responder run 
starts LLMNR and NBT-NS poisoning by default:

```text
# responder -I eth0
```

When a user in the same logical location as the attack host tries to access a non-existent shared drive. The drive 
has been made available and is asking for user credentials. Even if the user does not input credentials, the hashes 
will be obtained. Responder creates logs of every session. All the hashes dumped can be found in the folder 
`/usr/share/responder/logs`

2. Save the hashes in a file named `hash.txt` and use hashcat to crack it:

```text
# hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
```

* Module 5600 is the hash type to crack NetNTLMv2. 5500 is for NetNTLMv1/NetNTLMv1+ESS.

### LLMNR/NBT-NS poisoning through WPAD

1. Start responder with the NIC (for example `eth0`) and the options to configure a WPAD rogue proxy server (`w`) and
add the switch for DHCP injection (`d`:

```text
# responder -I eth0 -wd
```

When a user enters an invalid URL, the browser fails to load that page using DNS and sends out an LLMNR request to 
find a WPAD proxy server. This is by default in browsers that have "automatic configuration detection" enabled, 
an option often used in corporate networks to route traffic through a proxy. The browser then asks for `wpad.dat` which 
contains the proxy autoconfiguration data. 

Responder poisons and injects DHCP response with WPAD’s IP and the browser tries to authenticate to the WPAD server 
and gives a login prompt. When the user inputs credentials, the NLTM hashes are ours.
These can be viewed in the logs under the name HTTP-NTLMv2-<someIPv6Address>.txt

2. Use hashcat to crack it:

```text
# hashcat -m 5600 HTTP-NTLMv2-<someIPv6Address>.txt /usr/share/wordlists/rockyou.txt
```

* Module 5600 is the hash type to crack NetNTLMv2. 5500 is for NetNTLMv1/NetNTLMv1+ESS.

## Notes

### SMB

When a Windows system tries to access an SMB share, it sends a request to the DNS server which then resolves the 
share name to the respective IP address and the requesting system can access it. When the provided share name does 
not exist, the system sends out an LLMNR query to the entire network. If any user (IP address) has access to that 
share, it can reply.

### WPAD

Web Proxy Autodiscovery Protocol is a method used by a browser to automatically locate and interface with cache 
services in a network so that information is delivered quickly. WPAD by default uses DHCP to locate a cache service 
to facilitate straightforward connectivity and name resolution.

In an organisation using a WPAD server, each browser is provided with the same proxy configurations using a file 
called `wpad.dat`. Any request going from any browser in a company domain first finds `wpad.dat` and then reads the 
configuration and finally sends the request to the destination.

## Tools

* [Responder](https://github.com/lgandx/Responder)
* [Hashcat](https://www.kali.org/tools/hashcat/)

## Resources

* [rockyou.txt](https://github.com/redfiles/rockyou.txt)
* [Generic hash types](https://hashcat.net/wiki/doku.php?id=example_hashes)
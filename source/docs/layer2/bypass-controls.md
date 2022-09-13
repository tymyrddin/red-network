# Bypassing access controls

## Attack tree

```text
1 Spoof a source IP or MAC address of a valid device (macchanger)
2 Exploit implementation weaknesses
    2.1 Weak authentication mechanisms in wireless protocols (OR)
    2.2 Assign a static IP address instead of trying to get one through DHCP (OR)
    2.3 ...
3 Take advantage of NAC configuration weaknesses, for example, NAC enforces policy on IPv4 
  addresses but is not configured to protect IPv6 addresses
4 Use a rogue wireless access point
```

## Notes

 * Network access control (NAC), either in hardware or software, supports network visibility and access management 
 through policy enforcement on devices and users of corporate networks, and can quarantine rogue devices that are 
 not identified in a network security policy.
* In cloud-based environments, a customer can use Network Security Groups (NSG) or similar to enforce and control 
Internet or intranet communications across different workloads within virtual networks.

The most common ways to try to bypass NAC:

* Spoofing the MAC and IP addresses of a device that cannot natively participate in NAC, such as a VoIP phone or printer. These devices will be whitelisted by the administrator, and often there is no mechanism to verify that MAC address truly belongs to the device.
* Using IPv6 rather than IPv4 on the unauthorized device. Most servers have IPv6 addresses by default, and are running IPv6, but administrators still forget to include IPv6 rules in firewalls and NAC policy.
* Using a rogue wireless access point to get an authorized device to connect with an attacker machine. The attacker machine compromises the authorized device, then uses it to relay malicious traffic into the protected network.
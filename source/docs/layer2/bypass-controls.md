# Bypassing access controls

## Attack tree

```text
1 Spoof a source IP or MAC address of a valid device (macchanger)
2 Exploit implementation weaknesses
    2.1 Weak authentication mechanisms in wireless protocols (OR)
    2.2 Assign a static IP address on the network instead of trying to get one through DHCP (OR)
    2.3 ...
3 Take advantage of NAC configuration weaknesses, for example, NAC enforces policy on IPv4 addresses but is not 
configured to protect IPv6 addresses
```

## Notes

 * Network access control (NAC), either in hardware or software, supports network visibility and access management 
 through policy enforcement on devices and users of corporate networks, and can quarantine rogue devices that are 
 not identified in a network security policy.
* In cloud-based environments, a customer can use Network Security Groups (NSG) or similar to enforce and control 
Internet or intranet communications across different workloads within virtual networks.
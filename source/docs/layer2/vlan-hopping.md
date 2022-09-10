# VLAN hopping attacks

Using VLAN hopping attack, an attacker can sniff network traffic from another VLAN using a sniffer or send traffic 
from one VLAN to another VLAN.

## Attack tree

```text
1 VLAN hopping attack
    1.1 Switch spoofing 
    1.2 Double tagging
```

## Example

The additional security of a modern, tagged VLAN on the one hand depends on a header added to the
packet including the VLAN id. Such a packet can be easily created with Scapy.

```python
#!/usr/bin/python3

from scapy.all import Ether, Dot1Q, IP, ICMP

packet = Ether(dst="<MAC address>") / \
         Dot1Q(vlan=1) / \
         Dot1Q(vlan=2) / \
         IP(dst="<Target IP address>") / \
         ICMP()

sendp(packet)
```

## Notes

### Switch spoofing

Dynamic Trunking Protocol (DTP) is used to dynamically build trunk links between two switches. `dynamic desirable` 
(default), `dynamic auto` and `trunk` modes are used to configure an interface to allow dynamic trunking and frame 
tagging. 

A switch interface which is connected to an end device is normally in access mode and the end device will have 
access to its own VLAN. Traffic from other VLANs are not forwarded via the interface.

In switch spoofing, an adversary can generate Dynamic Trunking Protocol (DTP) messages to form a trunk link between 
the attack machine and the switch, as a result of a default configuration or an improperly configured switch.

If a switch is configured with the default values, and an adversary announces his/her attack machine is a trunk port, 
the switch will trunk all VLANs over the switch port that the attack machine is plugged into. The attacker
will now have access to all VLAN traffic destined for the switch.

### Double tagging

Double tagging happens when an adversary can connect to an interface which belongs to the native VLAN of 
the trunk port. Double tagging attack is unidirectional.

The attack takes advantage of 802.1Q tagging and the tag removal process of many types of switches. Many switches 
remove only one 802.1Q tag. In Double tagging attack, an attacker changes the original frame to add two VLAN tags: 
An outer tag of his own VLAN and an inner hidden tag of the victim's VLAN. The adversary's attack machine must 
belong to the native VLAN of the trunk link.

## Tools

* [Scapy](https://scapy.net/)

## Resources

* [802.1Q](https://standards.ieee.org/ieee/802.1Q/6844/)

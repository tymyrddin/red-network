# ARP cache poisoning

## Attack tree

```text
1 Craft a valid ARP reply in which any IP is mapped to any MAC address 
2 Broadcast this message. All the devices on network will accept this message and will update their ARP table with new Information
3 Gain control of the communication from any host in the network.
    3.1 Send an ARP reply mapping an IP address on network with a wrong or non-existent MAC address. For example, a fake ARP reply mapping the network router IP with a non-existent MAC will bring down the whole network.
    3.2 Send an ARP reply to the router mapping a particular host IP to your attack machine MAC address and another ARP reply to the host machine mapping the router IP to your attack machine MAC address. 
    3.3 Flood switch and sniff.
```
## Notes

All the devices that are connected to the layer 2 network have an ARP cache. This cache contains the mapping of all 
the MAC and IP address for the network devices that particular host has already communicated with.

### What is in a name

The terms [ARP spoofing](arp-spoofing.md) and ARP poisoning are generally used interchangeably. Technically, 
spoofing refers to an attacker impersonating another machine’s MAC address, while poisoning denotes the act of 
corrupting the ARP tables on one or more victim machines. In practice, these are both sub-elements of the same attack, 
and both terms are used to refer to the attack as a whole. Other similar terms might include ARP cache poisoning or 
ARP table corruption.

### Switches

Many network switches when overloaded can start acting like a hub and start broadcasting all the network traffic to 
all the hosts connected to the network. As a hub, the switch does not enable its port security feature, and now it 
broadcasts all the network traffic. Snifffff. 

### ARP cache poisoning likelihood

Poisoning ARP cache remotely is at minimum a 2-step exploitation chain, as it requires either physical access to the 
network or control of one of the machines in the network.
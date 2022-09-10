# Simple ARP spoofing

## Attack tree

```text
1 Use an ARP spoofing tool such as Arpspoof, Cain & Abel, Arpoison, or Ettercap
    1.1 Set the IP address of the tool to match the IP subnet of the victim (scans the network to find out 
        the IP address and MAC address of all the hosts on the subnetwork) (AND)
    1.2.Select a target (AND)
    1.3 Send ARP packet, replacing the MAC address of the target with own MAC address while keeping IP address 
        as is, causing packets meant for the target now being rerouted to the attacker (AND)
    1.4 When packets for the victim arrive, launch further attacks
        1.4.1 Associate multiple IP addresses to a single MAC address on a network (IP aliasing)
        1.4.2 Sit in between the communication between two users (MitM)
        1.4.3 Hijack session (network)
        1.4.4 Perform a DoS
```

## Example

```text
# echo 1 > /proc/sys/net/ipv4/ip_forward
# arpspoof -i <interface> -t <target IP address 1> <target IP address 2>
# arpspoof -i <interface> -t <target IP address 2> <target IP address 1>
```

## Notes

### What is in a name

The terms ARP Spoofing and ARP poisoning are generally used interchangeably. Technically, 
spoofing refers to an attacker impersonating another machine’s MAC address, while poisoning denotes the act of 
corrupting the ARP tables on one or more victim machines. In practice, these are both sub-elements of the same attack, 
and both terms are used to refer to the attack as a whole. Other terms used may be ARP cache poisoning or 
ARP table corruption.

### Adressing schemes

In an ARP spoofing attack, an adversary sends spoofed ARP messages over a LAN in order to link the adversary's MAC address with the IP address of a legitimate member of the network. Data that is intended for the host’s IP address gets sent to the adversary instead.
* ARP spoofing can be used to steal information, modify data-in-transit or stop traffic on a LAN.
* ARP spoofing attacks can also be used to facilitate other types of attacks, including [DoS](../tcp-ip/DoS.md) attacks, [session hijacking](../tcp-ip/Hijack-network-session.md) and [MitM](../box/MitM.md) attacks.

## Network tools
* [dsniff](https://www.monkey.org/~dugsong/dsniff/)
* [arpwatch](https://ee.lbl.gov/)



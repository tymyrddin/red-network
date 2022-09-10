# Introduction

## What?

Hacking Layer 2, responsible for addressing packets in an Ethernet with the use of MAC addresses.

## Why?

OSI was built to allow different layers to work without knowledge of each other. This means if one layer is hacked, 
communications are compromised without the other layers being aware of the problem. Security is only as strong as the 
weakest link, and when it comes to networking, layer 2 can be a VERY weak link.

There are two different addressing schemes for computers on a LAN, the global IP address and the local MAC address. 
The Address Resolution Protocol (ARP) was created to carry IP traffic. By merely injecting two ARP Reply packets into 
a trusting LAN, any device is able to receive all traffic going back and forth between any two devices on the LAN.

## How?

* [Simple ARP spoofing](arp-spoofing.md)
* [Network ARP cache poisoning](arp-cache-poisoning.md)
* [VLAN hopping attacks](vlan-hopping.md)


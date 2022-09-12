# Introduction

## What?

Name-resolution exploits are exploits against technologies that resolve names to IP addresses, such as DNS, which works 
at the application layer and provides essential lookup services for devices connected to the Internet or a private 
network, the NetBIOS name service that converts computer names to IP addresses, and the Link-local Multicast Name 
Resolution (LLMNR) protocol that converts hostnames to an IPv4 or IPv6 address. Common name-resolution ports are 
UDP port 137 (NetBIOS name service), UDP 138 (NetBIOS datagram service), and TCP port 139 (NetBIOS session service).

Hacking name resolution protocols is like hacking the telephone book of the internet or intranet. 

## Why?

DNS offers a simple and silent variant of the man-in-the-middle attack (on-path attack, as it is now called). Most 
of the time you only have to spoof a single DNS response packet to hijack all packets of a connection.

LLMNR is a protocol that allows name resolution without the requirement of a DNS server. It is able to provide a 
hostname-to-IP based off a multicast packet sent across the network asking all listening Network-Interfaces to reply 
if they are authoritatively known as the hostname in the query. You only have to respond authoritatively you can give 
what is asked after authentication.

## How?

* [PCAP files](pcap.md)
* [Sniffing](sniffing.md)
* [Forging and decoding packets](forging.md)



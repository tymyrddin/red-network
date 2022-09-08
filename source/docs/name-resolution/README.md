# Introduction

## What?

There are several protocols for hostname-to-IP resolutions: Domain Name System (DNS), Link-Local Multicast Name 
Resolution (LLMNR), and NetBIOS. 
* DNS works at the application layer and provides essential lookup services for devices connected to the Internet 
or a private network.
* LLMNR and NetBIOS are used by Microsoft operating systems to allow clients to help improve network communication 
efficiency and not send lookup requests outside network that can be resolved internally on the local area network 
(LAN). 

Hacking name resolution protocols is like hacking the telephone book of the internet or intranet. 

## Why?

DNS offers a simple and silent variant of the man-in-the-middle attack (on-path attack, as it is now called). Most 
of the time you only have to spoof a single DNS response packet to hijack all packets of a connection.

LLMNR is a protocol that allows name resolution without the requirement of a DNS server. It is able to provide a 
hostname-to-IP based off a multicast packet sent across the network asking all listening Network-Interfaces to reply 
if they are authoritatively known as the hostname in the query. You only have to respond authoritatively you can give 
what is asked after authentication.

## How?

* [DNS cache snooping](dns-snooping.md)
* [DNS spoofing](dns-spoofing.md)
* [DNS attacks overview](dns-attacks.md)
* [Attacking LLMNR and NetBIOS](llmnr-netbios.md)



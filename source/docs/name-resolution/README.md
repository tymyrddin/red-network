# Introduction

## What?

There are several protocols for hostname-to-IP resolutions. This section is dedicated to the Domain Name System (DNS). 
For Link-Local Multicast Name Resolution (LLMNR) and NetBIOS, see layer 2. DNS works at the application layer and 
provides essential lookup services for devices connected to the Internet or a private network.

Hacking DNS is like hacking the telephone book of the internet or intranet. 

## Why?

DNS offers a simple and silent variant of the man-in-the-middle attack. Most of the time you only have to spoof a 
single DNS response packet to hijack all packets of a connection.

## How?

1. [DNS spoofing](DNS-spoofing.md)
2. [DNS attacks](DNS-attacks.md)



# DNS attacks overview

## Attack tree

```text
1 DNS cache snooping
2 DNS poisoning
3 DNS amplification for DDoS attacks on other servers
    2.1 Create a botnet to send thousands of lookup requests to open DNS servers (with a spoofed source address 
    and configured to maximise the amount of data returned by each DNS server)
4 DDoS attack against a DNS server itself 
```

## Notes

* DNS cache poisoning, also known as DNS spoofing, exploits vulnerabilities in the domain name system DNS to divert Internet traffic away from legitimate servers and towards fake ones. This kind of attack is often used for pharming.
* DNS amplification attacks exploit the open nature of DNS services to strengthen the force of distributed denial of service [DDoS](../box/ddos.md) attacks.
* A DDoS attack against a DNS server can cause it to crash, rendering the users who rely on the sever unable to browse the web (users will still be able to reach recently visited websites if the DNS record is saved in a local cache).

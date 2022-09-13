# Attacks against SSL

## Attack tree

```text
1 SSL stripping
    1.1 Configure attack machine for IP forwarding (AND)
    1.2 Route all HTTP traffic to SSLStrip (AND)
    1.3 Run SSLStrip
2 SSL hijacking
3 SSL beast    
```

## Example

IP forwarding:

    # echo 1 > /proc/sys/net/ipv4/ip_forward

Route traffic

    # iptables –t nat –A PREROUTING -i eth0 –p tcp –dport 80 –j REDIRECT –to-port 54321

Run `sslstrip` and write the results to a file (`-w strip.log`), listening on port 54321 (`-l 54321`):

    # sslstrip -w strip.log -l 54321

## Notes

### SSL stripping

SSL stripping downgrades an HTTPS connection to HTTP by intercepting the TLS authentication sent from the 
application to the user. The adversary sends an unencrypted version of the application’s site to the user while 
maintaining the secured session with the application. 

It does not do any magical stuff to fulfill the job, it just replaces the protocol of all HTTPS
links in the sniffed traffic. The attacker must take care that the traffic of the victim flows
over his host by launching some kind of [on-path attack](../box/mitm.md) first.

### SSL hijacking

In SSL hijacking an adversary forges authentication keys and passes those to both the user and application during a 
TCP handshake. This sets up what appears to user and application to be a secure connection while the man in the 
middle controls the entire session. 

### SSL beast

SSL beast is an attack developed by Juliano Rizzo and Thai Duong, which leverages weaknesses in 
cipher block chaining (CBC) to exploit the Secure Sockets Layer (SSL) protocol. 
The CBC vulnerability can enable man-in-the-middle (MITM) attacks against SSL in order to silently decrypt 
and obtain authentication tokens, providing hackers with access to the data passed 
between a Web server and the Web browser accessing the server.

## Tools

* [dsniff](https://www.monkey.org/~dugsong/dsniff/)
* [sslstrip](https://www.kali.org/tools/sslstrip/)

## Resources

* [SSL BEAST](https://nerdoholic.org/uploads/dergln/beast_part2/ssl_jun21.pdf) (Browser Exploit Against SSL/TLS)
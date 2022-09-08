# DNS cache snooping

## Example

    nmap -sU -p 53 --script dns-cache-snoop.nse --script-args 'dns-cache-snoop.mode=timed,dns-cache-snoop.domains={host1,host2,host3}' <target>

Arguments:

* `dns-cache-snoop.mode`: which of two supported snooping methods to use. 
  * `nonrecursive`, the default, checks if the server returns results for non-recursive queries. Some servers may 
  disable this. 
  * `timed` measures the difference in time taken to resolve cached and non-cached hosts. This mode will pollute the 
  DNS cache and can only be used once reliably.
* `dns-cache-snoop.domains`: an array of domain to check in place of the default list. The default list of domains to 
check consists of the top 50 most popular sites, each site being listed twice, once with "www." and once without.

## Notes

The Nmap cache snooping script can assist with cache snooping against an internal DNS server. Cache snooping shows
where the targets are browsing on the Internet. This type of information disclosure can help aid in various attack 
scenarios:

* If knowing the websites the people (or a specific person) of an organisation frequents, a focused attack in which
the web pages in the site are infected with malware is a possible attack vector (Waterholing).
* An attack method used to impersonate a victim’s DNS server, forcing them to navigate to a malicious website 
(DNS spoofing).
* The DNS resolver cache is overwritten on the DNS server with a malicious web address, and the user will receive 
the malicious site instead of the intended one (DNS cache poisoning).

## Tools

* [dns-cache-snoop.nse](https://nmap.org/nsedoc/scripts/dns-cache-snoop.html)
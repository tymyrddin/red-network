# Overview common Wi-Fi attacks

## Attack tree

```text
    1 Sniff Wi-Fi traffic (AND)
    2 Gain access (OR)
        2.1 Connect to unencrypted network (OR)
        2.2 Connect to encrypted wireless network
            2.2.1 WPS attack
                2.2.1.1 Break WEP encryption
                    2.2.1.1.1 FMS attack
                    2.2.1.1.2 Korek's chop-chop
                    2.2.1.1.3 PTW attack
                    2.2.1.1.4 Caffe Latte attack
                2.2.1.2 Break WPA encryption
                    2.2.1.2.1 Beck-Tews attack
                    2.2.1.2.2 Halvorsen-Haugen attack
                    2.2.1.2.3 Brute force PMK
            2.2.2 ARP/MAC spoofing (MitM)
    3 Denial of Service
        3.1 Jam radio signal
        3.2 Flood with broadcast of frames
        3.3 Disassociation/deauthentication attack
```

## Notes

For many years now, ISPs deliver a router including an access point. And Wi-fi is integrated into more devices than 
just homes or company LANs. Every mobile phone or tablet has Wi-fi support nowadays. The VoIP infrastructure of some 
supermarket announcements are routed over Wi-fi. Advertising panels in buses, railways and at stations, and even 
surveillance cameras often use Wi-fi as a transport medium.

* Most users should now know that WEP is totally insecure or isn’t even configurable anymore, but on old routers they 
still are configurable and configured.
* The other protocols are also hackable, just not as extremely easy as WEP.
* Wi-fi is cheap, individually deployable and popular and therefore often built into the most unexpected places, blind 
to the associated massive security risks.
* In this evolving industry with ever more devices connected to wireless networks, understanding wireless security 
threats and countermeasures is critical.
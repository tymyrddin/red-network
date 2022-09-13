# Rogue access points

## Attack tree

```text
1 Evil twin attack (OR)
2 Karma attack (OR)
3 Captive portal
```

## Notes

### Evil twin attack

An evil twin attack occurs when an adversary sets up a fake access point that
impersonates a real access point for network users to access.

It is not always easy for a user to determine which is the correct Wi-Fi network and which is the fake. Both networks 
can have the same SSID and the same (or expected) encryption protocol and can be placed close to the targeted user(s) 
so that its signal strength is high, and it is put at the top of the client's list of APs. 

Using any kind of encryption protocol will require that the user knows the password, which may not be feasible. 
In which case, the evil twin is set to operate in open mode.

### Karma attack

Some devices, especially those running older operating systems, will send out active probe requests for known Wi-Fi 
networks rather than waiting passively for an AP to send a beacon frame. An adversary listening for such a request 
can respond with their own rogue AP information and prompt the client to connect.

In a karma attack, the adversary does not need to broadcast a spoofed SSID to entice users and potentially raise 
suspicion.

### Downgrade and SSL strip

Both Karma and Evil Twin can be used in combination with an on-path attack which intercepts all
traffic coming from the wireless client.

The connection with the server uses normal HTTPS. The connection with the client uses either a weaker version of SSL 
(downgrade attack, more easily cracked), or no encryption at all, using cleartext HTTP 
([SSL strip attack](../application/ssl-stripping.md)) between the hack machine and the client.

Both cases depend on the user permitting a connection to a website with an untrusted certificate. The certificate 
used in a downgrade attack is a self-signed certificate from the adversary machine. 

### Captive portal

A captive portal is the term for the web page that appears that asks users for their login credentials when 
connecting to a wireless network, most likely a guest wireless network, such as wireless networks found at airports or 
cafés. A captive portal attack occurs when the adversary sets up a captive portal for the evil twin
network that is used to prompt the user for the user’s password. Unsuspecting users may enter their passwords, 
not knowing they are connected to the fake Wi-Fi network. Once the adversary has the password, no time needs to 
be spent on capturing and cracking wireless traffic.

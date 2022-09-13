# Fake access points

## Attack tree

```text
1 Evil twin attack
2 Karma attack
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

The connection with the server uses normal HTTPS. The connection with the client uses either a weaker version of SSL 
(downgrade attack), or no encryption at all, using cleartext HTTP (SSL strip attack).

Both cases depend on the user permitting a connection to a website with an untrusted certificate. The certificate 
used in a downgrade attack is a self-signed certificate from the adversary machine. 

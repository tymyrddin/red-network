# Dragonblood attack

## Notes

WPA3 implements SAE (Dragonfly key exchange). The Extensible Authentication Protocol-Password (EAP-PWD) authentication 
method, which is used in some intranets, also uses Dragonfly handshakes and may be
vulnerable to some of the same attacks. 

It is vulnerable to multiple types of attacks, even despite the implementation
of additional security measures. WPA3 connections can be tricked into downgrading to
a weaker protocol (such as WPA2), or choose a weak security group using a rogue AP
and forged messages, or discern information about the password based on timing of
responses to commit frames.
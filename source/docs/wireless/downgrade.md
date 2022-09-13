# Downgrade and SSL strip

Both [Karma and Evil Twin](rogue-ap.md) can be used in combination with an on-path attack which intercepts all
traffic coming from the wireless client.

The connection with the server uses normal HTTPS. The connection with the client uses either a weaker version of SSL 
(downgrade attack, more easily cracked), or no encryption at all, using cleartext HTTP 
([SSL strip attack](../application/ssl-stripping.md)) between the hack machine and the client.

Both cases depend on the user permitting a connection to a website with an untrusted certificate. The certificate 
used in a downgrade attack is a self-signed certificate from the adversary machine. 

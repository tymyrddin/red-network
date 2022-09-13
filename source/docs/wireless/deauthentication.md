# Deauthentication attacks

## Attack tree

```text
1 Start capturing traffic
2 Deautheticate clients
3 Crack keys
```

## Example

```text
aireplay-ng -0 1 -a <MAC address of access point> -c <MAC address of target> wlan0mon
```

Following is a list of the parameters:

* `-0` tells Aireplay-ng to perform a deauthentication attack (you can also use --deauth).
* `1` specifies the number of deauthentication messages to send. You can use `0` for unlimited.
* `-a` is the MAC of the access point to send the message to.
* `-c` is the MAC address of the client to deauthenticate. If -c is not used, all clients are deauthenticated by the access point.
* `wlan0mon` is the interface to use.

## Notes

A deauthentication attack occurs when a hacker forces the access point to disconnect a wireless client from the 
access point. The client will automatically reconnect to the access point. Before deauthen-
ticating, the adversary will start capturing wireless traffic to capture the authentication traffic (the handshake) 
when the client reconnects to the access point. The captured traffic can help crack the encryption key.

## Tools

* [aireplay-ng](https://www.aircrack-ng.org/doku.php?id=aireplay-ng)
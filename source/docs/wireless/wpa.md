# Cracking WPA/WPA2 keys

## Attack tree

```text
1 Verify wireless NIC
2 Discover networks with Airodump-ng
3 Perform deauthentication attack
4 Crack the WPA/WPA2 key
```

## Example

### Verify wireless NIC

View and document wireless adapter:

```text
# airmon-ng
```

Create an interface that runs in monitor mode:

```text
# airmon-ng start wlan0
```
Write down interface name (something like `wlan0mon`)

### Discover networks with Airodump-ng

Display a list of wireless networks (Ctrl+C to stop):

```text
# airodump-ng wlan0mon
```

* The BSSID is the MAC address of the wireless access point that has been detected.
* PWR is the power level of the access point. The lower the number, the better the signal strength to that access point. 
This is a way to determine how close you are to the access point (unless the administrator changed the power level).
* CH is the channel the access point is operating on, such as 1, 6, or 11.
* ENC is encryption type used, such as WEP, WPA, or WPA2.
* CIPHER is the cipher being used, such as TKIP, CCMP, or WEP.
* ESSID is the name of the wireless network.

At the bottom of the output are the MAC addresses of the access points and the clients (stations) connected to those 
access points.

### Perform deauthentication attack

In a new terminal window, do a deauthentication attack on all clients connected::

```text
# aireplay-ng --deauth 0 -a <bssid of access point> wlan0mon
```

This allows the `airodump-ng` command running in the other terminal to
capture the handshake traffic when re-authentication happens. After a few minutes, switch back to the terminal where
`airodump-ng` is running, to view the the WPA handshake information that was captured (top
of the screen).

Switch back to the `aireplay-ng` terminal to stop the deauthentication traffic (Ctrl+C).

### Crack the WPA/WPA2 key

Crack the WPA/WPA2 encryption key using a brute-force method with a password list file:

```text
# aircrack-ng <filename.cap> -w <wordlist_file>
```

Once the password is cracked, a "KEY FOUND!" appears at the bottom of the
output followed by the encryption key. Copy this value and enter the key to connect to 
the wireless network.

Use Ctrl+C to stop any remaining commands in terminals.

## Tools

* [airmon-ng](https://aircrack-ng.org/doku.php?id=airmon-ng)
* [airodump-ng](https://aircrack-ng.org/doku.php?id=airodump-ng)
* [aireplay-ng](https://www.aircrack-ng.org/doku.php?id=aireplay-ng)
* [aircrack-ng](https://www.aircrack-ng.org/doku.php?id=aircrack-ng)

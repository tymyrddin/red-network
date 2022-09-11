# Using Wifite

## Attack tree

```text
Ridiculously simple
```

## Example

* In a terminal, type the `wifite` command to automate a wireless attack. The network card is placed in monitor mode, 
and then it scans for wireless networks. The wireless networks with the stronger signals are placed at the top.
* After two minutes of scanning for networks, press Ctrl+C to stop. 
* Type the number of the wireless network you would like to attack (assess). Wifite will attempt to crack the WPS pin 
and then continue by using tools such as `aircrack-ng` in order to attempt to crack the WPA2 encryption key.

## Notes

Use the `wifite -help` command to get a list of wifite possible switches. For example, if you just want to do the
WPS cracking with Wifite, use the -wps switch.

## Tools

* [wifite](https://tools.kali.org/wireless-attacks/wifite)

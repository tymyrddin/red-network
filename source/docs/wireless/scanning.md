# Scanning and sniffing

## Attack tree

```text
1 Scanning
2 Sniffing
3 Probe-Request sniffing
4 Sniffing for hidden SSID
```

## Examples

### Scanning

```python
#!/usr/bin/python3

from wifi import Cell

# Define the interface name that we will be sniffing from, you can
# change this if needed.
iface = "wlan0mon"

for cell in Cell.all(iface):
    output = "%s\t(%s)\tchannel %d\tsignal %d\tmode %s " % \
        (cell.ssid, cell.address, cell.channel, cell.signal, cell.mode)

    if cell.encrypted:
        output += "(%s)" % (cell.encryption_type.upper(),)
    else:
        output += "(Open)"

    print(output)

```

### Sniffing

```python
#!/usr/bin/python3

import os
from scapy.layers.dot11 import Dot11
from scapy.sendrecv import sniff

# Define the interface name that we will be sniffing from, you can
# change this if needed.
iface = "wlan0mon"
iwconfig_cmd = "/usr/sbin/iwconfig"

os.system(iwconfig_cmd + " " + iface + " mode monitor")


# Dump packets that are not beacons, probe request / responses
def dump_packet(pkt):
    if not pkt.haslayer(Dot11Beacon) and \
       not pkt.haslayer(Dot11ProbeReq) and \
       not pkt.haslayer(Dot11ProbeResp):
        print(pkt.summary())

        if pkt.haslayer(Raw):
            print(hexdump(pkt.load))
        print("\n")


while True:
    for channel in range(1, 14):
        os.system(iwconfig_cmd + " " + iface +
                  " channel " + str(channel))
        print("Sniffing on channel " + str(channel))

        sniff(iface=iface,
              prn=dump_packet,
              count=10,
              timeout=3,
              store=0)

```

### Probe-Request sniffing

```python
#!/usr/bin/python3

from datetime import datetime
import os
from scapy.layers.dot11 import Dot11
from scapy.sendrecv import sniff

# Define the interface name that we will be sniffing from, you can
# change this if needed.
iface = "wlan0mon"
iwconfig_cmd = "/usr/sbin/iwconfig"


# Print ssid and source address of probe requests
def handle_packet(packet):
    if packet.haslayer(Dot11ProbeResp):
        print(str(datetime.now()) + " " + packet[Dot11].addr2 +
              " searches for " + packet.info)


# Set device into monitor mode
os.system(iwconfig_cmd + " " + iface + " mode monitor")

# Start sniffing
print("Sniffing on interface " + iface)
sniff(iface=iface, prn=handle_packet)

```

### Sniffing for hidden SSID

```python
#!/usr/bin/python3

import os
from scapy.layers.dot11 import Dot11
from scapy.sendrecv import sniff

# Define the interface name that we will be sniffing from, you can
# change this if needed.
iface = "wlan0mon"
iwconfig_cmd = "/usr/sbin/iwconfig"


# Print ssid of probe requests, probe response
# or association request
def handle_packet(packet):
    if packet.haslayer(Dot11ProbeReq) or \
            packet.haslayer(Dot11ProbeResp) or \
            packet.haslayer(Dot11AssoReq):
        print("Found SSID " + packet.info)


# Set device into monitor mode
os.system(iwconfig_cmd + " " + iface + " mode monitor")

# Start sniffing
print("Sniffing on interface " + iface)
sniff(iface=iface, prn=handle_packet)

```

## Notes

### Scanning and sniffing

In contrast to a Wi-fi scanner a Wi-fi sniffer passively reads the network traffic and in the
best case evaluates also data frames beside beacon frames to extract information like SSID,
channel and client IPs/MACs.

### Probe-Request

If an operating system is sending out a probe request for every network it was connected to,
an adversary can not only conclude where its owner has been, but may even get the WEP key (if that 
is still used) when it tries to connect to these networks and only receives a probe response 
(it then reveals its WEP key).

### Hidden SSID

The Hidden SSID feature avoids adding the SSID to the Beacon frames, but does not make it invisible.
The SSID is also included in the probe request, the probe response and the association request packets. 
An adversary will only have to wait for a client and maybe disconnect it by sending a spoofed deauth.

## Tools

* [Scapy](https://scapy.net/)
* [PyPI wifi](https://pypi.org/project/wifi/)

## Resources

* [Wireless Client Sniffing with Scapy](https://www.sans.org/blog/special-request-wireless-client-sniffing-with-scapy/)
* [Linux wireless](https://wireless.wiki.kernel.org/welcome)
* [Cisko: type and subtype values for frames](https://community.cisco.com/t5/wireless-mobility-knowledge-base/802-11-frames-a-starter-guide-to-learn-wireless-sniffer-traces/ta-p/3110019)
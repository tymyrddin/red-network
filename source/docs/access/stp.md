# Attacking the spanning tree protocol

## Attack tree

```text
1 Capture STP packets on the LAN
2 Determine version (STP, RSTP, or MST) by inspecting the Bridge Protocol Data Units (BPDUs)
3 Craft malicious BPDUs of a nonexistent switch to elect it as the new root bridge
```

## Notes

In switched networks, when two network segments are connected by more than two layer 2 switches, this creates a 
physical switching loop in the topology, resulting in broadcast radiations and MAC table instability. Interconnecting 
the switches with redundant links helps some, but creates transmission loops.

The Spanning Tree Protocol (STP) is a layer 2 protocol that runs on network devices such as bridges and switches. 
Its primary function is to prevent looping in networks that have redundant paths by placing only one switch port in 
forwarding mode, and all other ports connected to the same network segment in blocking mode.

Bridge Protocol Data Units (BPDUs) are the update frames that are multicast between switches over the network 
regularly to determine if a port is in a forwarding or blocking state and to determine the root bridge during the 
election process.

## Sources

* [Attacking the Spanning-Tree Protocol](https://www.tomicki.net/attacking.stp.php)
* [Wireshark: ](https://wiki.wireshark.org/STP)
* [Cisco: Spanning Tree Protocol](https://www.cisco.com/c/en/us/tech/lan-switching/spanning-tree-protocol/index.html)
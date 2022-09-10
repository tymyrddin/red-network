# PCAP files

## Attack tree

```text
1 Capture packets
    1.1 tcpdump
    1.2 Wireshark
2 Analyse packets (packet tracing)
    2.1 tcpdump
    2.2 Wireshark
```

## Example

```text
# tcpdump -i <interface> -w <file-name>
# wireshark <filename>
```

## Notes

Packet Capture or PCAP (also known as libpcap) is an application programming interface (API) that captures live 
network packet data from OSI model Layers 2-7. Network analyzers like Wireshark create `.pcap` files to collect and 
record packet data from a network. 

## Resources

* [tcpdump](https://www.tcpdump.org/)
* [Wireshark](https://www.wireshark.org/)
# Bluetooth recon

## Attack tree

```text
1 Check bluetooth device is up
2 Scan
```
## Examples

### HCI

To verify you have a Bluetooth adapter:

    hciconfig

If the Bluetooth adapter is not enabled, enable it (where `hci0` is the interface ID):

    hciconfig hci0 up

To scan for Bluetooth devices (close to you):

    hcitool scan

Record the MAC address of a device in order to send commands to a device.

### Simple python scanner script

The `bt.discover_devices()` function returns a list of tuples with the first item being
the hardware address and the second contains the device name if the parameter
`lookup_names` is set to `True`, otherwise the return value is just a list of addresses.
Bluetooth makes an extra connection just to resolve every name.

```python
#!/usr/bin/python3

import bluetooth as bt

for (addr, name) in bt.discover_devices(lookup_names=True):
    print("%s %s" % (addr, name))
```

## Tools

* [Tools for Recon Bluetooth Devices](https://allabouttesting.org/tools-for-recon-bluetooth-devices-by-using-kali-linux/)
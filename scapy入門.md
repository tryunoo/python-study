## Index

## 1. はじめに
scapyは、パケットの作成、送信、解析を簡単に行うことが出来るpythonライブラリである。  
これを使用すれば、レイヤ2からパケットを作成することができる。  
また、pcapファイルなども簡単に解析することができる。
本稿では、パケットの作成、送信を行う。  

https://github.com/secdev/scapy


## 2. scapyのインストール
python2
```
% sudo pip install scapy
```

python3
```
% sudo pip3 install scapy-python3
```

MacOSでは、以下も必要
```
% brew install libdnet
```

## 3. scapy起動
scapyは、pythonプログラム中でインポートする他にも、コマンドを打つことにより、スタンダードpythonシェルまたは、IPythonで起動することが出来る。  
尚、パケットを送信するには、root権限が必要になる。  


### 3.1 スタンダードpythonシェルで起動
```sh
% scapy
WARNING: No route found for IPv6 destination :: (no default route?). This affects only IPv6
INFO: Please, report issues to https://github.com/phaethon/scapy
WARNING: IPython not available. Using standard Python shell instead.
Welcome to Scapy (3.0.0)
>>>
```

### 3.2 IPythonで起動
```sh
% sudo pip3 install ipython
% scapy
WARNING: No route found for IPv6 destination :: (no default route?). This affects only IPv6
INFO: Please, report issues to https://github.com/phaethon/scapy
Python 3.6.5 (default, Mar 30 2018, 06:41:53)
Type 'copyright', 'credits' or 'license' for more information
IPython 6.3.1 -- An enhanced Interactive Python. Type '?' for help.

Welcome to Scapy (3.0.0) using IPython 6.3.1
In [1]:
```

### 3.3インポートして使用
また、スクリプトにインポートする場合は以下のようにする。  
```sh
>>> from scapy.all import *
WARNING: No route found for IPv6 destination :: (no default route?). This affects only IPv6
>>>
```

## 4. パケットの生成

scapyには、各プロトコルがクラスで定義されており、以下のようにすることによって、パケットを生成することが出来る。  

```sh
>>> Ether()/IP(dst="192.168.1.1")/TCP(dport=80)
<Ether  type=0x800 |<IP  frag=0 proto=tcp dst=192.168.1.1 |<TCP  dport=http |>>>
```

scapyが対応しているプロトコルは、ls関数で表示することが出来る。  

```sh
>>> ls()
AH         : AH
ARP        : ARP
ASN1_Packet : None
BOOTP      : BOOTP
CookedLinux : cooked linux
DHCP       : DHCP options
...
```

また、各クラスがどのような値をとるかもls関数で調べることが出来る。  

```sh
>>> ls(Ether)
dst        : DestMACField         = (None)
src        : SourceMACField       = (None)
type       : XShortEnumField      = (36864)
>>> ls(IP)
version    : BitField             = (4)
ihl        : BitField             = (None)
tos        : XByteField           = (0)
len        : ShortField           = (None)
id         : ShortField           = (1)
flags      : FlagsField           = (0)
frag       : BitField             = (0)
ttl        : ByteField            = (64)
proto      : ByteEnumField        = (0)
chksum     : XShortField          = (None)
src        : Emph                 = (None)
dst        : Emph                 = ('127.0.0.1')
options    : PacketListField      = ([])
```


## 5. パケットの送信

### 5.1 送信系関数
パケットを送信する関数は、主に以下がある。  

| 名前  | 説明                                                    |
| ----- | ------------------------------------------------------- |
| send | レイヤ3でパケットを送信する                             |
| sendp | レイヤ2でパケットを送信する                             |
| sr    | レイヤ3でパケットを送信し、パケットを受信する           |
| sr1   | レイヤ3でパケットを送信し、最初のパケットのみを受信する |
| srp   | レイヤ2でパケットを送信し、パケットを受信する           |
| srp1  | レイヤ2でパケットを送信し、最初のパケットのみを受信する |

その他の関数については、以下を参照。  

[[Scapy] send recv系 使い方まとめ](http://python.zombie-hunting-club.com/entry/2018/04/18/112124)

### 5.2 レイヤ3で送受信
#### 5.2.1 send関数を使用して送信(受信はしない)
send関数を使用してicmpパケットを google.com に送信する。
```sh
send(IP(dst="google.com")/ICMP())
.
Sent 1 packets.
```

"Sent 1 packets." と言うメッセージを非表示にするには、verboseオプションをfalseに設定する。
```sh
>>> send(IP(dst="google.com")/ICMP(), verbose=False)
```

#### 5.2.2 sr1関数を使用して送受信
send関数では、返信を受信しないので、sr1関数を使用して受信する。

```sh
>>> sr1(IP(dst="google.com")/ICMP(), verbose=False)
<IP  version=4 ihl=5 tos=0x0 len=28 id=0 flags= frag=0 ttl=49 proto=icmp chksum=0xb4a2 src=172.217.26.14 dst=192.168.77.175 options=[] |<ICMP  type=echo-reply code=0 chksum=0xffff id=0x0 seq=0x0 |<Padding  load='\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' |>>>
```

#### 5.2.3 受信したパケットを表示
showメソッドを使用することによって、パケットの中身を見やすく表示することが出来る。
```sh
>>> r = sr1(IP(dst="google.com")/ICMP(), verbose=False)
>>> r.show()
###[ IP ]###
  version   = 4
  ihl       = 5
  tos       = 0x0
  len       = 28
  id        = 0
  flags     =
  frag      = 0
  ttl       = 49
  proto     = icmp
  chksum    = 0xb4a2
  src       = 172.217.26.14
  dst       = 192.168.77.175
  \options   \
###[ ICMP ]###
     type      = echo-reply
     code      = 0
     chksum    = 0xffff
     id        = 0x0
     seq       = 0x0
###[ Padding ]###
        load      = '\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
```

また、以下のようにすることで、特定のプロトコルや値を表示することが出来る。
```sh
>>> r["IP"]
<IP  version=4 ihl=5 tos=0x0 len=28 id=0 flags= frag=0 ttl=49 proto=icmp chksum=0xb4a2 src=172.217.26.14 dst=192.168.77.175 options=[] |<ICMP  type=echo-reply code=0 chksum=0xffff id=0x0 seq=0x0 |<Padding  load='\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' |>>>

>>> r["IP"].src
'172.217.26.14'
```

### 5.3 レイヤ2で送受信

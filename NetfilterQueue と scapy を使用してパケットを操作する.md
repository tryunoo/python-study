
## はじめに
Pythonのライブラリである NetfilterQueue を使用すると、ファイアウォールと組み合わせてパケットをインターセプトすることが出来る。  
また、インターセプトしたパケットは好きに変更することが出来る。
本稿では、NetfilterQueue と scapy を使用して ping の応答パケットを傍受、変更してみる。

## インストール

netfilterqueue をインストールする。
```
$ sudo pip3 install netfilterqueue
...
netfilterqueue.c:437:42: fatal error: libnfnetlink/linux_nfnetlink.h: No such file or directory
compilation terminated.
error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
```

エラーが出た。  
どうやら linux_libnfnetlink.h と言うものが足りないらしいので、インストールする。

```
$ sudo apt install libnetfilter-queue-dev
```

再度実行する。

```
$ sudo pip3 install netfilterqueue
...
    100% |████████████████████████████████| 61kB 1.1MB/s
Installing collected packages: netfilterqueue
  Running setup.py install for netfilterqueue ... done
Successfully installed netfilterqueue-0.8.1
```
インストールできた。


## ファイアウォールの設定
netfilterqueue を使用して、パケットを操作するには、Netfilterの設定をしなくてはならない。  
以下では、iptablesコマンドを使用してキュー1に受信したicmpパケットを入れる設定を施している。

```
# iptables -A INPUT -p icmp -j NFQUEUE --queue-num 1
# iptables -nL
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
NFQUEUE    icmp --  0.0.0.0/0            0.0.0.0/0            NFQUEUE num 1

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination  
```

## NetfilterQueueを使用してみる

```python
# test.py
from netfilterqueue import NetfilterQueue


def callback(packet):
  print(packet.get_payload_len()) # パケットの長さを表示
  print(packet.get_payload()) # パケットの中身をバイト型で表示

  packet.accept() # パケットを許可する

  # パケットをドロップする場合は以下
  # packet.drop()


def main():
  nfqueue = NetfilterQueue()

  nfqueue.bind(1, callback) # キューID 1 にバインド

  try:
    nfqueue.run()
  except KeyboardInterrupt:
    pass

  nfqueue.unbind()


if __name__ == '__main__':
  main()
```

上記を実行し、pingを送信する。

```
$ ping google.com -c 1
PING google.com (172.217.25.78) 56(84) bytes of data.
64 bytes from nrt13s50-in-f14.1e100.net (172.217.25.78): icmp_seq=1 ttl=49 time=2.83 ms

--- google.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 2.832/2.832/2.832/0.000 ms
```

すると以下の出力になり、受信したパケットをみることが出来る。
```
# python3 test.py
84
b'E\x00\x00T\x00\x00\x00\x001\x01\xb7s\xac\xd9\x19N\n\x00\x02\x0f\x00\x00\xba\xcf e\x00\x01\x01\x15\xd8Z\x00\x00\x00\x00\x86\x87\x06\x00\x00\x00\x00\x00\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./01234567'
```

## パケットを操作する
以下は、受信したパケットの送信元をグーグルからヤフーに変更してホストに渡すプログラムである。

```python
from netfilterqueue import NetfilterQueue
from scapy.all import *


def callback(packet):
  pkt = IP(packet.get_payload())

  ip = IP(src="yahoo.co.jp", dst=pkt["IP"].dst)
  icmp = ICMP(type=pkt["ICMP"].type, id=pkt["ICMP"].id, seq=pkt["ICMP"].seq)
  raw = pkt["Raw"]

  packet.set_payload(bytes(ip/icmp/raw))

  packet.accept()


def main():
  nfqueue = NetfilterQueue()

  nfqueue.bind(1, callback)

  try:
    nfqueue.run()
  except KeyboardInterrupt:
    pass

  nfqueue.unbind()


if __name__ == '__main__':
  main()
```

上記を実行し、google.com に ping を送信すると以下のようになり、f1.top.vip.ssk.yahoo.co.jp から応答があったかのように見える。

```
$ ping google.com -c 1
PING google.com (172.217.25.78) 56(84) bytes of data.
64 bytes from f1.top.vip.ssk.yahoo.co.jp (182.22.59.229): icmp_seq=1 ttl=64 time=3.01 ms

--- google.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 3.018/3.018/3.018/0.000 ms
```

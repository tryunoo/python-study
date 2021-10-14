CentOS7にPython3.6をインストールし、pipを使えるようにする。  

## Index

[:contents]

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-5634140305449664"
     data-ad-slot="3588425951"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 1. Python3.6のインストール

### 1.1 インストール手順
Python3系は、yumのデフォルトのリポジトリに入っていないので、[IUS Community Project](https://ius.io/)のリポジトリを追加する必要がある。  
IUSは、PythonやPHPの最新バージョンの RPM packages を配布しているコミュニティである。  

```
$ sudo yum install -y https://centos7.iuscommunity.org/ius-release.rpm
```

Python3.6と適当なパッケージをインストールする。  
```
$ sudo yum install -y python36u python36u-devel python36u-libs
```

### 1.2 インストールの確認
python3.6がインストールされたかタブ補完で確認する。  
```
$ python
python      python2     python2.7   python3.6   python3.6m  
```

また、上記にある python3.6m は、--with-pymalloc オプションを設定されたものである。  
このオプションは、小さなオブジェクト（512バイト以下）に最適化されたPython専用のメモリアロケータを使用するもので、
メモリの割当を効率よく行えるようになる。  
pymalloc アロケータについては[こちら](https://docs.python.jp/3/c-api/memory.html#the-pymalloc-allocator)を参照


### 1.3 名前の変更
python3.6 を python3 と打っても実行出来るようにする。
```
$ which python3.6
/usr/bin/python3.6
$ sudo cp /usr/bin/python3.6 /usr/bin/python3
$ python3 --version
Python 3.6.2
```

## 2. pipのインストール

### 2.1 pipのインストール方法
PIPのインストール方法は2つある。  
まずは、yumでインストールする方法である。

```
$ sudo yum install -y python36u-pip
```

もうひとつは、[ensurepip](https://docs.python.jp/3/library/ensurepip.html) モジュールを使用する方法である。
```
$ sudo python3.6 -m ensurepip
```

このモジュールは、インターネットにアクセスしないとドキュメントに記されている。  
どうやらpython3.6にはpipのインストーラも同胞されており、
このモジュールによってpipがインストールされるらしい。  

また、インストールされているpipのバージョンを更新するには、以下のコマンドを入力すればよい。   
```
$ python -m ensurepip --upgrade
```

### 2.2 pipの実行方法
pipの実行方法も二種類ある。  

ひとつは、pipコマンドを使用する方法である。
```
$ pip3 --version
pip 9.0.1 from /usr/lib/python3.6/site-packages (python 3.6)
```

もうひとつはpythonコマンドのモジュール指定で実行する方法である。
```
$ python3 -m pip --version
pip 9.0.1 from /usr/lib/python3.6/site-packages (python 3.6)
```

コマンドは上記のどちらでも良いと思うが、タイプ数の少ない前者を使用すればいいような気がする。

## Index


[:contents]


<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- ad1 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-5634140305449664"
     data-ad-slot="5763163158"
     data-ad-format="auto"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 1. はじめに
PyQueryは、HTML をJQueryライクに扱えるようにするライブラリである。  
スクレイピングをする上で同じようなことは、BeautifulSoup でもできるが、BeautifulSoupでは、勝手に整形される（タグを閉じていないと閉じられるなど）ので、それが嫌なときなどに使用することができる。  
（この機能を無効にする方法を知っている方がいれば教えていただきたい）  
また、個人的に PyQuery のほうがシンプルにコードを書けると感じているが、機能は BeautifulSoup のほうが多い。
BeautifulSoup の使用方法については、以下の記事を参考にしたい。  
([Python] BeautifulSoup で Webスクレイピングをしてみる)[http://python.zombie-hunting-club.com/entry/2017/11/08/192731]   

本稿では、PyQueryを使用して HTML から情報を抽出する方法を説明する。  

## 2. インストール
```
% sudo pip install pyquery
```

## 3. 使用するhtml
今回使用する HTML は以下になる。  

```python
from pyquery import PyQuery as pq

html = """
<!DOCTYPE html>
<html>
  <head>
  </head>
  <body>

    <h1 id="test-id" class="test-class">Test Text</h1>

    <a href="http://zombie-hunting-club.com">ゾンビ狩りクラブ</a>

    <ul>
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
    </ul>

    <div id="div1">
        <p class="p1">pタグ内のテキスト1</p>
    </div>

    <div id="div2">
        <p class="p2">pタグ内のテキスト2</p>
    </div>

  </body>
</html>
"""
```

今回は直接文字列に書き込んでいるが、どっかのサイトから持ってくる場合は、以下のようにインスタンスを作成するか、Requests や selenium + Phantom.js なんかで HTML を取得する。  
```python
pq(url='http://google.com')
```

## 4. PyQueryインスタンスの生成
PyQueryインスタンスを生成することで、操作が行えるようになる。  

```python
query = pq(html, parser='html')
print(type(query))
# <class 'pyquery.pyquery.PyQuery'>

print(query)
# <html>
#   <head>
#   </head>
#   <body>
#
#     <h1 id="test-id" class="test-class">Test Text</h1>
#
#     <a href="http://zombie-hunting-club.com">ゾンビ狩りクラブ</a>
#
#     <ul>
#         <li>item1</li>
#         <li>item2</li>
#         <li>item3</li>
#     </ul>
#
#     <div id="div1">
#         <p class="p1">pタグ内のテキスト1</p>
#     </div>
#
#     <div id="div2">
#         <p class="p2">pタグ内のテキスト2</p>
#     </div>
#
#   </body>
# </html>
```

## 5. 要素の検索
要素の検索は、多くがJQueryの構文と同じであるので、ここでは一部を紹介する。  

### 5.1 タグから検索  
タグから検索する方法は、以下の2種類ある。

```python
query.find("h1")
query("h1")
```

```python
print(type(query("h1")))
# <class 'pyquery.pyquery.PyQuery'>

print(query("h1"))
# <h1 id="test-id" class="test-class">Test Text</h1>
```

### 5.2 idから検索
```python
print(query("#test-id")) # id
# <h1 id="test-id" class="test-class">Test Text</h1>
```

### 5.3 classから検索
```python
print(query(".test-class")) # class
# <h1 id="test-id" class="test-class">Test Text</h1>

print(query("#div1 > p"))
# <p class="p1">pタグ内のテキスト1</p>
```

### 5.4 テキストから検索
```python
print(query("h1:contains('Test Text')"))
# <h1 id="test-id" class="test-class">Test Text</h1>
```

### 5.5 URLから検索
完全一致
```python
print(query("a[href = 'http://zombie-hunting-club.com']"))
# <a href="http://zombie-hunting-club.com">ゾンビ狩りクラブ</a>
```

URLに "zombie" が含まれるものに一致
```python
<a href="http://zombie-hunting-club.com">ゾンビ狩りクラブ</a>
# <a href="http://zombie-hunting-club.com">ゾンビ狩りクラブ</a>
```

## 6. 取得
### 6.1 テキストを取得
```python
print(query("h1").text())
# Test Text
```

### 6.2 id や class, URL などの属性を取得
```python
print(query("h1").attr("id"))
# test-id

print(query("h1").attr("class"))
# test-class

print(query("a").attr('href'))
# http://zombie-hunting-club.com
```
または、
```python
print(query("h1").attr.id)
# test-id

print(query("h1").attr.class_)
# test-class
```

### 6.3 n番目の要素を取得
```python
print(query("p"))
# <p class="p1">pタグ内のテキスト1</p>
# <p class="p2">pタグ内のテキスト2</p>

print(query("p").eq(0))
# <p class="p1">pタグ内のテキスト1</p>

print(query("p").eq(1))
# <p class="p2">pタグ内のテキスト2</p>
```


### 6.4 子要素の取得
```python
print(query("div").children())
# <p class="p1">pタグ内のテキスト1</p>
# <p class="p2">pタグ内のテキスト2</p>
```

## 7 複数要素がマッチした場合
以下のように、複数マッチする場合、for文を使用することができる。  

```python
print(query("p"))
# <p class="p1">pタグ内のテキスト1</p>
# <p class="p2">pタグ内のテキスト2</p>

for x in query("p"):
    text = pq(x).text()
    print(text)
# pタグ内のテキスト1
# pタグ内のテキスト2
```

## 8. filter
要素をフィルタすることができる。  
```python
print(query("p").filter(lambda x: "テキスト1" in pq(this).text())) # テキストに "テキスト1" が入っている要素のみ抽出する
# <p class="p1">pタグ内のテキスト1</p>
```

## 参考
- https://pythonhosted.org/pyquery/api.html  
- https://media.readthedocs.org/pdf/pyquery/latest/pyquery.pdf  

## 書籍
[asin:4873117380:detail]

[asin:4774142298:detail]

[asin:477415539X:detail]

[asin:B00XZTYMG6:detail]

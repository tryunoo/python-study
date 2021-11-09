## Index


## 1. はじめに
BeautifulSoup は、HTML や XML のパーサであり、Webスクレイピングで使用することができる。  
また、同じようにスクレイピングをする他のライブラリには、PyQueryがあるが、そちらは以下の記事を参照したい。  
[[Python] PyQuery でWebスクレイピングをしてみる](http://python.zombie-hunting-club.com/entry/2017/11/07/225538)  

本稿では、BeautifulSoup4 を使用して HTML から情報を抽出する方法を説明する。

## 2. インストール
```
% pip install beautifulsoup4
```

## 3. 使用するhtml

```python
from bs4 import BeautifulSoup

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
今回は直接文字列に書き込んでいるが、どっかのサイトから持ってくる場合は、Requests や selenium + Phantom.js なんかで HTML を取得する。  

## 4. インスタンスの生成
以下でインスタンスを生成することができる。  
また、prettifyメソッドを使用して html を整形して表示することができる。  

```python
soup = BeautifulSoup(html, "html.parser")

print(type(soup))
# <class 'bs4.BeautifulSoup'>

print(soup.prettify()) # htmlを整形して表示
# <!DOCTYPE html>
# <html>
#  <head>
#  </head>
#  <body>
#   <h1 class="test-class" id="test-id">
#    Test Text
#   </h1>
#   <a href="http://zombie-hunting-club.com">
#    ゾンビ狩りクラブ
#   </a>
#   <ul>
#    <li>
#     item1
#    </li>
#    <li>
#     item2
#    </li>
#    <li>
#     item3
#    </li>
#   </ul>
#   <div id="div1">
#    <p class="p1">
#     pタグ内のテキスト1
#    </p>
#   </div>
#   <div id="div2">
#    <p class="p2">
#     pタグ内のテキスト2
#    </p>
#   </div>
#  </body>
# </html>
```

## 5. 検索メソッド
検索メソッドには、以下がある。

- find_all() と find()
- select()
- find_all_next() と find_next()
- find_all_previous() と find_previous()
- find_next_siblings() と find_next_sibling()
- find_previous_siblings() と find_previous_sibling()
- find_parent() と find_parents()


## 6. find() と find_all()
スクレイピングを行う上では、この２つのメソッドがメインになる。  
find() と find_all() の書式は同じで、find()はマッチしたものの最初の一つを返すが、find_all()はすべてをリストで返す。  

また、find_all() メソッドのショートカットである soup() も使用することができる。  

書式は以下になる。  
```python
find_all(name, attrs, recursive, string, limit, **kwargs)
find(name, attrs, recursive, string, **kwargs)
```

## 7. 要素の検索
### 7.1 タグから検索
```python
print(soup.find("p"))
# <p class="p1">pタグ内のテキスト1</p>
```
また、タグなら以下でも検索することができる。  
```python
print(soup.p)
# <p class="p1">pタグ内のテキスト1</p>
```

マッチしたものすべて返すようにするには、以下のようにする。  
```python
print(soup.find_all("p"))
# [<p class="p1">pタグ内のテキスト1</p>, <p class="p2">pタグ内のテキスト2</p>]
```
もしくは、soup() を使用して以下のように検索することができる。
```python
print(soup("p"))
# [<p class="p1">pタグ内のテキスト1</p>, <p class="p2">pタグ内のテキスト2</p>]
```

### 7.2 idから検索
idを指定して検索する方法は以下がある。  
```python
print(soup(id="test-id"))
# [<h1 class="test-class" id="test-id">Test Text</h1>]

print(soup.find_all(id="test-id"))
# [<h1 class="test-class" id="test-id">Test Text</h1>]

print(soup.find_all(attrs={"id":"test-id"}))
# [<h1 class="test-class" id="test-id">Test Text</h1>]
```

### 7.3 classから検索
```python
print(soup.find_all(class_="test-class"))
# [<h1 class="test-class" id="test-id">Test Text</h1>]
```

### 7.4 URLから検索
```python
print(soup.find_all(href="http://zombie-hunting-club.com"))
# [<a href="http://zombie-hunting-club.com">ゾンビ狩りクラブ</a>]
```

### 7.5 テキストから検索
```python
print(soup.find_all(string="pタグ内のテキスト2"))
# ['pタグ内のテキスト2']
```

### 7.6 複数条件を指定して検索
```python
print(soup.find_all("h1", id="test-id", string="Test Text", attrs={"class":"test-class", "id":"test-id"}))
# [<h1 class="test-class" id="test-id">Test Text</h1>]
```

## 8. 取得
### 8.1 タグ名を取得
```python
print(soup.find(class_="p1").name)
# p
```

### 8.2 id や class, URL などの属性を取得
getメソッドで取得することができる。  

```python
print(soup.div.get("id"))
# div1

print(soup.p.get("class"))
# ['p1']

print(soup.a.get("href"))
# http://zombie-hunting-club.com
```

### 8.3 テキストを取得
```python
print(soup.a.string)
# ゾンビ狩りクラブ
```
```python
print(soup.find(class_="p2").string)
# pタグ内のテキスト2
```

## 9. selectメソッド
selectメソッドを使用すれば、以下のようにcssセレクタのように検索が行える。

```python
print(soup.select("div > .p2"))
# [<p class="p2">pタグ内のテキスト2</p>]
```


## 10. その他のメソッド
### 10.1 find_all_next() と find_next()
```python
find_all_next(name, attrs, string, limit, **kwargs)
find_next(name, attrs, string, **kwargs)
```

要素以降の指定した要素を検索する。
```python
h1 = soup.h1
print(h1)
# <h1 class="test-class" id="test-id">Test Text</h1>

print(h1.find_all_next("p"))
# [<p class="p1">pタグ内のテキスト1</p>, <p class="p2">pタグ内のテキスト2</p>]
```

### 10.2 find_all_previous() と find_previous()
```python
find_all_previous(name, attrs, string, limit, **kwargs)
find_previous(name, attrs, string, **kwargs)
```
find_all_next() と find_next() の要素以前を検索するバージョン。

### 10.3 find_next_siblings() と find_next_sibling()
```python
find_next_siblings(name, attrs, string, limit, **kwargs)
find_next_sibling(name, attrs, string, **kwargs)
```

要素以降の同じ深さにある指定した要素を検索する。
```python
h1 = soup.h1
print(h1)
# <h1 class="test-class" id="test-id">Test Text</h1>

print(h1.find_next_siblings("div"))
# [<div id="div1">
# <p class="p1">pタグ内のテキスト1</p>
# </div>, <div id="div2">
# <p class="p2">pタグ内のテキスト2</p>
# </div>]
```

```python
h1 = soup.h1
print(h1.find_next_siblings("li"))
# []
```

### 10.4 find_previous_siblings() と find_previous_sibling()
```python
find_previous_siblings(name, attrs, string, limit, **kwargs)
find_previous_sibling(name, attrs, string, **kwargs)
```
find_next_siblings() と find_next_sibling() の要素以前を検索するバージョン。  

### 10.5 find_parent() と find_parents()
```python
find_parents(name, attrs, string, limit, **kwargs)
find_parent(name, attrs, string, **kwargs)
```
親要素を返す。  
```python
p = soup.find("p")
print(p)
# <p class="p1">pタグ内のテキスト1</p>

print(p.find_parents("div"))
# [<div id="div1">
# <p class="p1">pタグ内のテキスト1</p>
# </div>]
```

## 参考
- https://www.crummy.com/software/BeautifulSoup/bs4/doc/


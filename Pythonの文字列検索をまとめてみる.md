## Index


## 1. はじめに
Pythonの文字列の検索または、マッチするかを調べる方法をまとめる。  
Pythonの文字列を調べる方法は、主に以下の３種類ある。  

- 演算子を用いる
- 文字列のインスタンスを用いる
- 正規表現のインスタンスを用いる

## 2. 演算子を用いる方法

| 演算子 | 説明                                                                       |
|:-------|:---------------------------------------------------------------------------|
| a == b | a と b がメモリ上の同じアドレスならば True を、そうでなければ False を返す |
| a is b | a と b が同じ文字列なら True を、そうでなければ False を返す               |
| a in b | b に a が含まれるのなら True を、そうでなければ False を返す               |

```python
# test.py
print("abc" == "abc")
print("abc" is "abc")
print("bc" in "abcd")
```
```
% python3 test.py
True
True
True
```

## 3. 文字列のメソッドを用いる方法
文字列を検索する文字列メソッドを示す。  
また、次に示すメソッドで使用されている文字の意味は以下になる。

| 文字             | 意味                                                        |
|:-----------------|:------------------------------------------------------------|
| str              | 検索対象文字列                                              |
| sub              | 検索する文字列                                              |
| start および end | 文字列のスライス s[start:end]の範囲で検索することを意味する |
| prefix           | 接頭語の意                                                  |
| sufix            | 接尾語の意                                                  |


| 文字列メソッド                         | 説明                                                                                                                          |
|:---------------------------------------|:------------------------------------------------------------------------------------------------------------------------------|
| str.find(sub[, start[, end]])          | sub が含まれる場合、その最小のインデックスを返す<br>sub が見つからなかった場合 -1 を返す                                      |
| str.index(sub[, start[, end]])         | find() と同様だが、部分文字列が見つからなかったとき ValueError を送出する                                                     |
| str.rfind(sub[, start[, end]])         | sub が含まれる場合、その最大のインデックスを返す<br>sub が見つからなかった場合 -1 を返す                                      |
| str.rindex(sub[, start[, end]])        | rfind() と同様だが、部分文字列が見つからなかったとき ValueError を送出する                                                    |
| str.startswith(prefix[, start[, end]]) | 文字列が指定された prefix で始まるなら True を、そうでなければ False を返す<br> prefix は見つけたい複数の接頭語のタプルでも可 |
| str.endswith(suffix[, start[, end]])   | suffix で終わるなら True を、そうでなければ False を返す<br> suffix は見つけたい複数の接尾語のタプルでも可                    |
| str.count(sub[, start[, end]])         | sub が重複せず出現する回数を返す                                                                                              |


```python
# test.py
s = """
Name: Elvis Presley
TEL: 090-1234-5678
Born: January 8, 1935
Describing from wikipedia: Elvis Aaron Presley[a] (January 8, 1935 – August 16, 1977) was an American singer and actor. Regarded as one of the most significant cultural icons of the 20th century, he is often referred to as the "King of Rock and Roll" or simply "the King".
"""

sub = 'Elvis'

print(s.find(sub))
print(s.index(sub))
print(s.rfind(sub))
print(s.rindex(sub))
print(s.startswith(sub))
print(s.endswith(sub))
print(s.count(sub))
```

```bash
% python3 test.py
7       # find()
7       # index()
89      # rfind()
89      # rindex()
False   # startswith()
False   # endswith()
2       # count()
```


```python
# test.py
s = """
Name: Elvis Presley
TEL: 090-1234-5678
Born: January 8, 1935
Describing from wikipedia: Elvis Aaron Presley[a] (January 8, 1935 – August 16, 1977) was an American singer and actor. Regarded as one of the most significant cultural icons of the 20th century, he is often referred to as the "King of Rock and Roll" or simply "the King".
"""

sub = 'ABCDE'

print(s.find(sub))
print(s.index(sub))
print(s.rfind(sub))
print(s.rindex(sub))
print(s.startswith(sub))
print(s.endswith(sub))
print(s.count(sub))
```

```
% python3 test.py
-1
Traceback (most recent call last):
  File "test.py", line 24, in <module>
    print(s.index(sub))
ValueError: substring not found
```

## 4. 正規表現のメソッドを用いる方法
文字列を検索する正規表現メソッドを示す。  
また、次に示すメソッドで使用されている文字の意味は以下になる。

| 文字    | 意味             |
|:--------|:-----------------|
| pattern | 検索する正規表現 |
| string  | 検索対象文字列   |


| 正規表現メソッド                       | 説明                                                                                                     |
|:---------------------------------------|:---------------------------------------------------------------------------------------------------------|
| re.search(pattern, string, flags=0)    | 正規表現がマッチする最初の場所を探して、match オブジェクト を返す<br>マッチしない場合は、None を返す     |
| re.match(pattern, string, flags=0)     | 文字列の先頭で正規表現にマッチすれば、match オブジェクトを返す<br>マッチしない場合は、None を返す        |
| re.fullmatch(pattern, string, flags=0) | 文字列全体が正規表現にマッチすれば、match オブジェクトを返す<br>マッチしない場合は、None を返す          |
| re.findall(pattern, string, flags=0)   | 重複しないすべてのマッチを文字列のリストとして見つかった順で返す<br>マッチしない場合は、空のリストを返す |
| re.finditer(pattern, string, flags=0)  | findall()と同様だが、結果としてイテレータを返す                                                          |


```python
# test.py
s = """
Name: Elvis Presley
TEL: 090-1234-5678
Born: January 8, 1935
Describing from wikipedia: Elvis Aaron Presley[a] (January 8, 1935 – August 16, 1977) was an American singer and actor. Regarded as one of the most significant cultural icons of the 20th century, he is often referred to as the "King of Rock and Roll" or simply "the King".
"""

pattern = '\d{4}'

print(re.search(pattern, s))
print(re.match(pattern, s))
print(re.fullmatch(pattern, s))
print(re.findall(pattern, s))
print(re.finditer(pattern, s))
```

```bash
% python3 test.py
<_sre.SRE_Match object; span=(30, 34), match='1234'>
None
None
['1234', '5678', '1935', '1935', '1977']
<callable_iterator object at 0x7fb85e6a1c88>
```

### match オブジェクトの扱い
match オブジェクトは、以下のメソッドを持つ。

| メソッド | 説明                                         |  |
|:---------|:---------------------------------------------|:-|
| group()  | 正規表現にマッチした文字列を返す             |  |
| start()  | マッチの開始位置を返す                       |  |
| end()    | マッチの終了位置を返す                       |  |
| span()   | マッチの位置 (start, end) を含むタプルを返す |  |

```python
# test.py
s = """
Name: Elvis Presley
TEL: 090-1234-5678
Born: January 8, 1935
Describing from wikipedia: Elvis Aaron Presley[a] (January 8, 1935 – August 16, 1977) was an American singer and actor. Regarded as one of the most significant cultural icons of the 20th century, he is often referred to as the "King of Rock and Roll" or simply "the King".
"""

pattern = '\d{4}'

res = re.search(pattern, s)

print(res.group())
print(res.start())
print(res.end())
print(res.span())
```

```bash
% python3 test.py
1234
30
34
(30, 34)
```


## 参考
- https://docs.python.jp/3/library/stdtypes.html  
- https://docs.python.jp/3/library/re.html  

## 書籍

[asin:4873117380:detail]

[asin:4774142298:detail]


[asin:477415539X:detail]

[asin:B00XZTYMG6:detail]

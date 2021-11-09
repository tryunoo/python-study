## Index



## 1. はじめに
pythonのRequestsを使えば、簡単にHTTPリクエストを作成することができる。  
本稿では、Requestsの基本的な使い方を紹介する。  

## 2. インストール
Requestsは、pipでインストールできる。  
```
% pip install requests
```

また、以下でインポートする。  
```python
import requests
```

## 3. リクエストの生成
Requestsでは、以下7メソッドを使用してリクエストを生成することができる。  
戻り値は、Response オブジェクトである。  

```python
requests.request(method, url, **kwargs)
```

また、method が指定されている以下のメソッドでも実現できる。
```python
requests.get(url, **kwargs)
requests.post(url, **kwargs)
requests.put(url, **kwargs)
requests.delete(url, **kwargs)
requests.head(url, **kwargs)
requests.options(url, **kwargs)
```

### オプション  

- **params**  
辞書またはバイナリ  
URLのクエリ文字列として送信される  

- **data**  
辞書、バイナリ、またはファイルオブジェクト  
ボディに埋め込まれ送信される  

- **headers**  
HTTPヘッダの辞書  
ヘッダとして送信される  

- **cookies**  
辞書または CookieJar オブジェクト  
クッキーとして送信される  

- **files**  
名前とファイルオブジェクトの辞書 ( {名前:ファイルオブジェクト} )  
または、( {名前 : (ファイル名:ファイルオブジェクト)} )  
multipart エンコードしファイルをアップロードする  

- **auth**  
認証タプル  
Basic/Digest/Custom HTTP 認証用  

- **timeout**  
Float  
リクエストのタイムアウトを指定する  

- **allow_redirects**  
Boolean  
POST/PUT/DELETE リダイレクトを許可するならTrue

- **proxies**  
次のような辞書  
{'http': 'http://127.0.0.1:1234'}  

- **verify**  
Boolean  
真ならば、SSL証明書を検証する  

- **stream**  
Boolean  
False の場合、レスポンスの内容が直ちにダウンロードされる  

- **cert**  
文字列またはタプル  
文字列の場合、ssl クライアント証明書のパス  (.pem)  
タプルの場合、('cert', key') のペア  

## 4. ステータスコードの取得
```python
r = requests.get("http://httpbin.org/get")
print(r.status_code)
# 200
```

## 5. レスポンスヘッダの取得
```python
r = requests.get("http://httpbin.org/get")
print(r.headers)
# {'X-Powered-By': 'Flask', 'Connection': 'keep-alive', 'X-Processed-Time': '0.000687837600708', 'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true', 'Server': 'meinheld/0.6.1', 'Via': '1.1 vegur', 'Content-Length': '267', 'Date': 'Mon, 06 Nov 2017 01:49:32 GMT'}
```

上記は普通の辞書に見えるが、参照時に大文字と小文字を区別しない。  
```python
print(r.headers["Content-Type"])
# 'application/json'

print(r.headers["CONTENT-TYPE"])
# 'application/json'
```

また、getメソッドを使用しても要素を取得することができる。  

```python
print(r.headers.get("Content-Type"))
# 'application/json'

print(r.headers.get("content-type"))
# 'application/json'
```

## 6. ボディの取得
### 6.1 テキストを取得
```python
r = requests.get("http://httpbin.org/get")
print(r.text)
# {
#   "args": {},
#   "headers": {
#     "Accept": "*/*",
#     "Accept-Encoding": "gzip, deflate",
#     "Connection": "close",
#     "Host": "httpbin.org",
#     "User-Agent": "python-requests/2.18.4"
#   },
#   "origin": "118.103.21.193",
#   "url": "http://httpbin.org/get"
# }

```

### 6.2 bytesとして取得
```python
print(r.content)
# b'{\n  "args": {}, \n  "headers": {\n    "Accept": "*/*", \n    "Accept-Encoding": "gzip, deflate", \n    "Connection": "close", \n    "Host": "httpbin.org", \n    "User-Agent": "python-requests/2.18.4"\n  }, \n  "origin": "x.x.x.x", \n  "url": "http://httpbin.org/get"\n}\n'
```

### 6.3 jsonとして取得
```python
r = requests.get("http://httpbin.org/get")
print(r.json())
# {'url': 'http://httpbin.org/get', 'origin': 'x.x.x.x', 'headers': {'Accept-Encoding': 'gzip, deflate', 'Host': 'httpbin.org', 'Accept': '*/*', 'User-Agent': 'python-requests/2.18.4', 'Connection': 'close'}, 'args': {}}

print(r.json()["url"])
# 'http://httpbin.org/get'
```

## 7. 画像データを取得、保存する
```python
import requests
from PIL import Image
from io import BytesIO

r = requests.get('http://httpbin.org/image/png')
image = Image.open(BytesIO(r.content))
image.save("test.png")
```


## 8. getのパラメータ追加
```python
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get("http://httpbin.org/get", params=payload)
print(r.url)
# 'http://httpbin.org/get?key1=value1&key2=value2'
```

## 9. リクエストヘッダの変更
```python
headers = {'user-agent':'test'}
r = requests.get("http://httpbin.org/get", headers=headers)
print(r.json()['headers']['User-Agent'])
# 'test'
```

## 10. フォームの送信
HTMLのフォームのようなデータを送信するには、data 引数に辞書を渡せば良い。  

```python

data = {'key1': 'value1', 'key2': 'value2'}
r = requests.post("http://httpbin.org/post", data=data)
print(r.json()['form'])
# {'key1': 'value1', 'key2': 'value2'}
```

エンコードされていないデータを送信したい場合は、文字列を渡せば良い。  
```python
import json
r = requests.post("http://httpbin.org/post", data=json.dumps(data))
print(r.json()['data'])
# '{"key1": "value1", "key2": "value2"}'
```

## 参照
http://requests-docs-ja.readthedocs.io/en/latest/user/quickstart/

## 書籍
[asin:4873117380:detail]

[asin:4774142298:detail]

[asin:477415539X:detail]

[asin:B00XZTYMG6:detail]

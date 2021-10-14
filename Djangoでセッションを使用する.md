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
本稿では、Django での session 管理をして、ログインやログアウトの実装をしてみる。  

## 2. 環境
```bash
% python3 --version
Python 3.5.2
% python3 -m django --version
1.10.8
```

## 3. ファイル構成
```bash
% ls
manage.py  myapp/  mysite/

% ls mysite
__init__.py  __pycache__/  settings.py  urls.py  wsgi.py

% ls myapp
__init__.py   admin.py  migrations/  tests.py  views.py
__pycache__/  apps.py   models.py    templates/  urls.py
```

## 4. セッション有効設定
mysite/setting.py に以下の記述があるのを確認する（なければ追加する）。  

```
INSTALLED_APPS = [
  ...
  'django.contrib.sessions',
  ...
]

MIDDLEWARE = [
  ...
  ‘django.contrib.session.middleware.SessionMiddleware',
  ...
]
```
また、必要に応じて以下を実行する。  
```
% python3 manage.py migrate
```

## 5. セッション操作

### セッションにデータを保存する
```python
request.session['name'] = name
```

### セッションにデータがあるか確認する
```python
if 'name' in request.session:
```

### セッションからデータを読み込む
```python
name = request.session['name']
```

### セッションのデータを削除する
```python
del request.session['name']
```

### セッションをクリアする
```python
request.session.clear()
```

### セッションとクッキーを削除する
```python
request.session.flush()
```

## 6. サンプルコード
viewsは以下のようにする。  
```python
# myapp/views.py
from django.shortcuts import render

def index(request):
    loggedIn = False
    name = None

    if 'name' in request.POST and 'passwd' in request.POST:
        request.session['name'] = request.POST['name']
        request.session['passwd'] = request.POST['passwd']

    if 'logout' in request.POST:
        request.session.clear()

    if 'name' in request.session and 'passwd' in request.session:
        name = request.session['name']
        loggedIn = True

    return render(request, "index.html", {'loggedIn':loggedIn, 'name':name})
```

また、index.htmlを作成する。  
```html
<!-- myapp/templates/index.html -->
<!DOCTYPE html>
<htm>
  <head>
  </head>

  <body>
    <form action="" method="post">
      {% csrf_token %}
      {% if not loggedIn %}
      <input type="text" name="name" required />
      <input type="password" name="passwd" required />
      <input type="submit" value="submit">
      {% else %}
      <p>Hello {{ name }}!</p>
      <input type="submit" value="logout" name="logout" />
      {% endif %}
    </form>

  </body>
</html>
```

実行して、localhost:8000 にアクセスすると、以下の画面が表示される。  
[f:id:Garfields:20171106222328p:plain]

ログインしてみる
[f:id:Garfields:20171106222226p:plain]

また、ログアウトボタンを押すと、セッションを削除し、ログアウトすることができる。  

## 関連記事
http://python.zombie-hunting-club.com/index#Web

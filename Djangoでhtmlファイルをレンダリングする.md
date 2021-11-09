## Index


## 1. はじめに
本稿では、Djangoでhtmlファイルをレンダリングする方法を説明する。  
Djangoのインストールや基本的な使い方は以下の記事を参考にしたい。
[[Python] Django 入門](http://zhc-python.hatenablog.com/entry/2017/11/03/223435)  

## 2. ファイル構成
mysiteプロジェクトの構成は以下になる。  
```
% ls
manage.py  myapp/  mysite/

% ls mysite
__init__.py  __pycache__/  settings.py	urls.py  wsgi.py

% ls myapp
__init__.py   admin.py	migrations/  static/	 tests.py  views.py
__pycache__/  apps.py	models.py    templates/  urls.py
```

myapp 内で今回使用するのもは以下になる。  

- views.py  
ビューを記述する
- urls.py  
urlを記述する
- templates/  
htmlファイルを格納する
- static/  
cssやJavaScriptなどのhtmlで読み込ませるファイルを配置する

## 3. 設定
アプリケーションを認識させるために、setting.py の INSTALLED_APPS に設定を施す必要がある。  
myapp アプリケーションを認識させるには、myapp/apps.py に記述されている MyappCofing クラスを INSTALLED_APPS に追加する。  

```python
# mysite/settings.py

...
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp.apps.MyappConfig', # ここ
]
...
```

## 4. htmlファイルの作成
htmlファイルは、myapp/templates/ 配下に配置する。

```
% ls myapp/templates
index.html
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="ja">
  <head>
  </head>
  <body>

    <h1>Hello from index.html</h1>

  </body>
</html>
```


## 5. ビューの作成
htmlファイルをレンダリングするには、render関数を使用する。

```python
from django.shortcuts import render

def index(request):
    return render(request, "index.html")
```

上記のようにすることで、templates/index.html を読み込み、レンダリングすることができる。  

## 6. URLの設定
URLの設定をする。  

```python
# mysite/urls.py
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^', include('myapp.urls')),
]
```

```python
# myapp/urls.py
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^index/', views.index, name='index'),
]
```

設定したURLにアクセスする。  
localhost:8000

[f:id:Garfields:20171018222556p:plain]

## 7. cssやJavaScriptの読み込み
cssやJavaScriptファイルは、myapp/static ディレクトリ配下に配置する。

```
% ls myapp/static
index.css  index.js
```


```css
/* index.css */
h1 {
  color: red;
}
```

```javascript
// index.js
window.onload = function() {
  var e = document.getElementById('test');

  e.style.color = 'blue';
}
```

staticディレクトリ配下のファイルを読み込むには、まず {% load static %} でロードする必要がある。  
次に以下のように記述することで、各ファイルを読み込むことができる。

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="ja">
  <head>
    {% load static %}
    <link rel="stylesheet" href="{% static 'index.css' %}" />
    <script src="{% static 'index.js' %}"></script>
  </head>
  <body>

    <h1>Hello</h1>
    <h2 id='test'>World</h2>

  </body>
</html>
```

これを実行した画面が以下になる。
[f:id:Garfields:20171018222626p:plain]

## 関連記事
http://python.zombie-hunting-club.com/index#Web

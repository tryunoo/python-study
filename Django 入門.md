## Index



## 1. はじめに
[Django](http://djangoproject.jp/about/)は、Python用のフルスタックWebフレームワークであり、データベース操作やユーザ認証、セッション管理などのWebアプリケーション開発に必要な機能を簡単に実装できるように提供している。  
Djangoは非常に多くの機能を持っているので、機能の少ない小さいWebアプリケーションの開発には、[Flask](http://www.zombie-hunting-club.com/entry/2017/10/09/164156)を使用するのも良いだろう。  

今回は、Djangoを使用して、簡単なWebサーバを構築する。  

また、Pythonは3.6を使用している。  
```
$ python3 --version
Python 3.6.2
```

## 2. Djangoのインストール
pipでDjangoをインストールする  
```
$ sudo pip3 install django
```

バージョンを確認  
```
$ python3 -m django --version
1.11.6
```

## 3. プロジェクトの作成
Djangoのプロジェクトを作成するには、以下のコマンドを実行する。  
```
$ django-admin startproject mysite
```
mysiteの部分は、プロジェクトの名前であり、その名前のディレクトリが作成される。  
名前は基本何でもよいが、django や test という名前は、Pythonパッケージ名と衝突するので、避けるべきである。  
また、うまく動作しない場合は、[Problems running django-admin](https://docs.djangoproject.com/ja/1.11/faq/troubleshooting/#troubleshooting-django-admin) を参照したい。  


*django-admin startproject* コマンドを実行すると、以下のようなファイルが自動で作成される。  
```
$ ls
mysite
$ ls mysite/*
mysite/manage.py

mysite/mysite:
__init__.py  settings.py  urls.py  wsgi.py
```

各ファイルは次のようになっている。  

| ファイル名                  | 説明                                                                                                                                                       |
|:----------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| mysite/                     | プロジェクトが入っているディレクトリ。この名前は動作に影響しないので、好きに変更できる                                                                   |
| mysite/manage.py            | Djangoに対するさまざまな操作を行うユーティリティプログラム                                                                                                 |
| mysite/mysite/              | プロジェクトの本当のPythonパッケージ                                                                                                                       |
| mysite/mysite/\_\_init__.py | このディレクトリがPythonパッケージであることを知らせる空ファイル。<br>これがあることによって、このディレクトリにあるPythonプログラムをimportで呼び出せる |
| mysite/mysite/settings.py   | Djangoプロジェクトの設定ファイル                                                                                                                           |
| mysite/mysite/urls.py       | サイトのURL構成を記述するファイル                                                                                                                          |
| mysite/mysite/wsgi.py       | WSGI互換Webサーバーとのエントリーポイント                                                                                                                  |

## 4. アプリケーションの作成
アプリケーションを作成するには、以下のコマンドを実行する。  
```
$ python3 manage.py startapp [アプリケーション名]
```

実際に、mysiteディレクトリに入り、*myapp* というアプリケーションを作成する。  
```
$ cd mysite/
$ ls
manage.py  mysite
$ python3 manage.py startapp myapp
$ ls
manage.py  myapp  mysite
$ ls -1 myapp
__init__.py
admin.py
apps.py
migrations
models.py
tests.py
views.py
```
myappの中身は以下のようになっている。  

| ファイル名    | 説明                                                               |
|:--------------|:-------------------------------------------------------------------|
| \_\_init__.py | このディレクトリがPythonパッケージであることを知らせる空ファイル。 |
| admin.py      | データベースをweb画面上で管理できるようにするファイル              |
| apps.py       | アプリケーションの管理に使用される                                 |
| migrations    | データベースのモデルの作成や変更に使用される                       |
| models.py     | データベースのモデルを記述する                                     |
| tests.py      | テストコードを記述するファイル                                     |
| views.py      | ビューの管理をする                                                 |

## 5. ビューの作成
ここでは、実際にweb画面に表示するものを作成する。  
以下は、Web画面上に "Hello" と表示するだけの簡単なビューを作成するコードである。  

```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello")
```

## 6. URLの設定
ビューを作成したが、これを表示させるには、URLを設定しなければならない。  
urlの設定方法は、2つある。  
ひとつは以下のように mysite/urls.py に、myapp.view をインポートする方法である。  

```python
from django.conf.urls import url
from django.contrib import admin
import myapp.views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$', myapp.views.index, name='index')
]
```

もうひとつは、myapp/urls.py ファイルを作成し、それを mysite/urls.py でインクルードする方法である。  
```python
# myapp/urls.py
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
```

```python
# mysite/urls.py
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^', include('myapp.urls')),
]
```

## 7. 言語設定（オプション）
mysite/settings.py から言語やタイムゾーンを変更することができる。  

```python
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'UTC'
```
上記を以下に変更する。  
```python
LANGUAGE_CODE = 'ja'
TIME_ZONE = 'Asia/Tokyo'
```

## 8. Djangoを動かす
実際に動かすには、プロジェクトディレクトリ内で以下のコマンドを入力する。

```
% ls
manage.py  myapp/  mysite/

% python3 manage.py runserver
```

サーバが立ち上がるので、localhost:8000 にアクセスしてみと、きちんとページが表示できていることが確認できる。

[f:id:Garfields:20171018212219p:plain]


また、デフォルトでは、デバッグモードが有効になっており、エラーが出た時に詳細情報を出力する。  
例えば、存在しないURLにアクセスした場合は、以下の画面になる。  

[f:id:Garfields:20171018212227p:plain]

これを表示したくない場合は、mysite/settings.py にある以下の記述を次のように書き換えればよい。
```
#DEBUG = True
#ALLOWED_HOSTS = []

DEBUG = False
ALLOWED_HOSTS = ['*']
```

デバッグモードを無効にして存在しないURLにアクセスした場合は以下のようになる。

[f:id:Garfields:20171018212242p:plain]

## 関連記事
http://python.zombie-hunting-club.com/index#Web

## 参考
- http://djangoproject.jp/
- https://docs.djangoproject.com/ja/1.11/intro/


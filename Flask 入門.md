## Index


## 1 .はじめに

[Flask](http://flask.pocoo.org)はPythonのWebアプリケーションフレームワークであり、Djangoのようなフルスタックフレームワークと違い、軽量で簡単に実行できる。  
今回は、Flaskを使用した簡単なWebアプリケーションを作成する。  

## 2. pipでFlaskをインストールする

```bash
% sudo pip3 install flask
% python3 --version
Python 3.6.2
% python3
Python 3.6.2 (default, Jul 17 2017, 16:44:45)
[GCC 4.2.1 Compatible Apple LLVM 8.1.0 (clang-802.0.42)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import flask
>>> flask.__version__
'0.12.2'
```

## 3. 最小構成で実行してみる

### 3.1 ディレクトリ構成
```bash
% pwd
/work/flask-project
% ls
server.py
```

### 3.2 コード
```python
# server.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello'


if __name__ == '__main__':
    app.debug = True
    app.run(host='0.0.0.0', port=80)
```

@app.route('/') でurlのパスとマッピングする。  
上記では、index関数をルートurlとマッピングしている。  

### 3.3 プログラム実行
```
% python3 server.py
 * Running on http://0.0.0.0:8080/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 669-169-150
127.0.0.1 - - [09/Oct/2017 13:04:12] "GET / HTTP/1.1" 200 -
```

上記のように実行し、127.0.0.1:8080 に接続すると、"Hello" と表示される。

[f:id:Garfields:20171009133633p:plain]

## 4. htmlファイルを表示する

3章では、Webページに "Hello" という文字列を index 関数の戻り値として返すことで表示した。  
では、実際のhtmlファイルを表示したいときはどうするか。  
それは render_template 関数を戻り値にすることで実現できる。  
htmlファイルは、templatesディレクトリに配置する必要がある (templates以外の名前だと動作しない) 。  
また、render_template をインポートする必要がある。  

### 4.1 ディレクトリ構成
```bash
% ls
__pycache__	server.py	templates

% ls templates
index.html
```

### 4.2 コード
```python
# server.py
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html')


if __name__ == '__main__':
    app.debug = True
    app.run(host='0.0.0.0', port=8080)
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

### 4.3 実行画面
先ほどと同じように接続すると、以下のような画面が表示される。  

[f:id:Garfields:20171009133908p:plain]


## 5. CSSとJavaScriptを読み込む

### 5.1 ディレクトリ構成
以下の様な構成にし、staticディレクトリ配下にCSSファイルとJavaScriptファイルを配置する。  
staticという名前でないと動作しない。  
```
% ls
__pycache__	server.py	static		templates
[r00t@Echelon] ~/work/flask-project
% ls static
index.css	index.js
```

### 5.2 コード
CSSやJavaScriptを読み込むには、以下の様に{{ url_for() }} を使用する。
```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="ja">
  <head>
    <link rel="stylesheet" href="{{url_for('static', filename='index.css')}}">
    <script src="{{url_for('static', filename='index.js')}}"></script>
  </head>
  <body>

    <h1>Hello</h1>
    <h2 id='test'>World</h2>

  </body>
</html>
```

```css
/* index.css */
h1 {
  color: red;
}
```

```JS
// index.js
window.onload = function() {
  var e = document.getElementById('test');

  e.style.color = 'blue';
}
```

### 5.3 実行画面
[f:id:Garfields:20171009164144p:plain]

## 6. pythonの値をhtmlに埋め込む

Flask は [Jinja2](http://jinja.pocoo.org/docs/2.9/) というテンプレートエンジンを使用でき、htmlファイルに簡単にpythonの値を埋め込むことができる。  

### 6.1 Jinja2インストール
```bash
sudo pip3 install jinja2
```

### 6.2 コード
```python
# server.py
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def index():

    s = "abc"
    lis = ["a1", "a2", "a3"]
    dic = {"name":"John", "age":24}
    bl = True

    return render_template('index.html', s=s, lis=lis, dic=dic, bl=bl)


if __name__ == '__main__':
    app.debug = True
    app.run(host='0.0.0.0', port=8080)
```

render_template の第二引数以降に値を指定することで、値を渡すことが出来る。  


```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="ja">
  <head>
  </head>
  <body>
    <p>{{ s }}</p>
    <hr>

    <ul>
    {% for x in lis %}
      <li>{{ x }}</li>
    {% endfor %}
    </ul>
    <hr>

    <p>{{ "name: %s age: %s" % (dic["name"], dic["age"]) }}</p>
    <hr>

    {% if bl %}
      <p>True</p>
    {% else %}
      <p>False</p>
    {% endif %}
  </body>
</html>
```

### 6.3 実行画面
[f:id:Garfields:20171009152947p:plain]

## 7. GET/POSTで値を渡す

### 7.1 コード
以下の index.html ではボタンを二つ作成し、/test に get と post リクエストを投げられるようにしている。  

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="ja">
  <head>
  </head>
  <body>

    <form action="/test" method="get">
      <button name="get_value" value="from get">get submit</button>
    </form>

    <form action="/test" method="post">
      <button name="post_value" value="from post">post submit</button>
    </form>

  </body>
</html>
```

GET/POST で値を取得するには、request をインポートする。  

```python
from flask import Flask, render_template, request

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/test', methods=['GET', 'POST'])
def test():
    if request.method == 'GET':
        res = request.args.get('get_value')
    elif request.method == 'POST':
        res = request.form['post_value']

    return res


if __name__ == '__main__':
    app.debug = True
    app.run(host='0.0.0.0', port=8080)
```

@app.route('/test', methods=['GET', 'POST'])  
で GET と POST の受け取りを指定している。  
GET だけ受け取るときは、methods=['GET'] のようにする。  

また、get の値を取得するには、request.args.get　関数を、post の値を取得するには、request.form から取得出来る。  

### 7.2 実行画面
index.html
[f:id:Garfields:20171009161716p:plain]

get submit ボタンを押した時の画面
[f:id:Garfields:20171009161739p:plain]

post submit ボタンを押した時の画面
[f:id:Garfields:20171009161748p:plain]

## 関連記事
http://python.zombie-hunting-club.com/index#Web

## 書籍

[asin:1491991739:detail]

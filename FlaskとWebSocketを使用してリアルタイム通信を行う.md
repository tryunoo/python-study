今回は、FlaskとWebSocketを使用してターミナルに打った文字列をWeb画面に表示する。
Flaskの基本的な使い方については以前の記事を参照されたい。  
[[Python] Flask 入門](http://zhc-python.hatenablog.com/entry/2017/11/03/223503)

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

## 1. 環境構築

PythonでWebSocketを使用するには、[gevent](http://www.gevent.org/index.html)というライブラリが使えるので、インストールする。

```
sudo pip3 install flask
sudo pip3 install gevent
```

## 2. コード

```python
# server.py
from flask import Flask, request, render_template
from gevent import pywsgi
from geventwebsocket.handler import WebSocketHandler

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/pipe')
def pipe():
    if request.environ.get('wsgi.websocket'):
        ws = request.environ['wsgi.websocket']

        while True:
            ws.send(input())


def main():
    app.debug = True
    server = pywsgi.WSGIServer(("", 8080), app, handler_class=WebSocketHandler)
    server.serve_forever()


if __name__ == "__main__":
    main()
```

```html
<!-- index.html -->
<html>
    <head>
        <script type="text/javascript">
            var ws = new WebSocket("ws://localhost:8080/pipe");

            ws.onmessage = function(e) {
              document.getElementById("text-field").innerHTML = e.data;
            }
        </script>
    </head>
    <body>
        <h1>WebSocket Example</h1>
        <p id="text-field"></p>
    </body>
</html>
```

## 3. 実行画面

[f:id:Garfields:20171010221546g:plain]

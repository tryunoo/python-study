## Index



## 1. はじめに

[plotly](https://plot.ly)は、データ解析、可視化を行うツールであり、APIライブラリも提供している。  
ライブラリはPythonやJavaScript、R言語などで使用することができる。  

Plotlyを使用すると、以下のようなグラフが簡単にプロットすることができる。  
範囲選択でズーム、ダブルクリックで元に戻すことができる。

<div>
    <a href="https://plot.ly/~PewResearch/4/" target="_blank" title="Percentage of American Adults 18+ Who Own Each Device" style="display: block; text-align: center;"><img src="https://plot.ly/~PewResearch/4.png" alt="Percentage of American Adults 18+ Who Own Each Device" style="max-width: 100%;width: 800px;"  width="800" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="PewResearch:4" src="https://plot.ly/embed.js" async></script>
</div>


また、以下のような3Dグラフもプロットすることができる。
<div>
    <a href="https://plot.ly/~jackp/5564/" target="_blank" title="untitled (10)" style="display: block; text-align: center;"><img src="https://plot.ly/~jackp/5564.png" alt="untitled (10)" style="max-width: 100%;width: 840px;"  width="840" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="jackp:5564" src="https://plot.ly/embed.js" async></script>
</div>

他にも様々なグラフが[公式サイト](https://plot.ly)に載っている。

本稿では、Python用のAPIライブラリを使用して、基本的なグラフをプロットする方法を説明する。  
plotlyには、オンラインとオフラインがあり、  
オンラインでは、作成したグラフがクラウドに保存される。これを使用するには、アカウント登録が必要である。  

plotlyは、pipでインストールすることができる。  
```
% python3 --version
Python 3.6.2
% sudo pip3 install plotly
% python3 -q
>>> plotly.__version__
'2.1.0'
```

## 2. オンラインで使ってみる

### 2.1 アカウント取得
まずは、アカウントを作成する。  
アカウントの作成は[ここ](https://plot.ly/accounts/login/?action=login)から行える。  
また、plotlyには、有料版も存在し、保存できるチャートの数などが変わる。

ユーザを作成したら、"settings" の "API keys" から自分のAPIキーを確認できる。

### 2.2 APIの設定
APIを設定するために、ユーザ名とAPIキーを入力し以下を実行する。

```python
>>> import plotly
>>> plotly.tools.set_credentials_file(username='', api_key='')
```

すると、以下のファイルにユーザ名とAPIキーが書き込まれ、APIの設定が完了する。

```
% cat ~/.plotly/.credentials
{
    "stream_ids": [],
    "proxy_username": "",
    "api_key": "ひみつだよ",
    "username": "ひみつだよ",
    "proxy_password": ""
}
```

### 2.3 plotでグラフをプロットしてみる
オンラインでプロットするためのメソッドは以下の2つある。

- plotly.plotly.plot()  
ユニークなurlを返して、urlを自動で開くこともできる。
- plotly.plotly.iplot()  
jupyter Notebookを使用している場合にノートブックにプロットする。


```python
import plotly.plotly as py
from plotly.graph_objs import *

data1 = Scatter(
    x = [1, 2, 3, 4, 5, 6, 7, 8, 9],
    y = [1, 2, 3, 4, 5, 6, 7, 8, 9],
    name = 'データ1'
)
data2 = Scatter(
    x = [8, 6, 4, 2],
    y = [16, 5, 11, 9],
    name = 'データ2'
)

data = Data([data1, data2])

py.plot(data, filename='figure-name')
```

[f:id:Garfields:20171017230613p:plain]

### 2.4 plotメソッドのオプション引数
- filename (string)  
グラフに紐付けられた名前
- fileopt ('new' | 'overwrite' | 'extend' | 'append')  
  - 'new'  
  新たなユニークなurlでプロットする  
  - 'overwrite'  
  filenameで紐付けられているファイルを上書きする  
  - 'extend'  
  現在グラフに存在するデータセットに値を追加する  
  - 'append'  
  現在グラフに存在するデータリストにデータセットを追加する  
- auto_open (default=True)  
  - True  
  プロットしたページを新しいタブで自動で開く  
  - False  
  プロットしたページを開かず、URLのみ返す  
- sharing ('public' | 'private' | 'secret')  
  - 'public'  
  作成したグラフは誰でも観覧することができる。プロファイル、検索エンジンにも表示される。  
  - 'private'  
  作成者のみ観覧可能。Plotlyフィードやプロファイル、検索エンジンに表示されない。観覧するには、ログインする必要がある。作成したグラフは、他のPlotlyユーザへプライベートにシェアすることができる。  
  - 'secret'  
  秘密のリンクを知っているものが観覧することができる。Plotlyフィードやプロファイル、検索エンジンに表示されない。ぐらふがwebページやIPythonノートブックに埋め込まれている場合は、誰でも観覧することができる。  
- world_readable (default=True)  
推奨されていない。sharingを使用すべし。グラフをprivate/publicにする。  

### 2.5 iplotでグラフをプロットしてみる
上記コードの py.plot だったところを py.iplot にするだけでよい。

[f:id:Garfields:20171017230426p:plain]

### 2.6 iplotメソッドのオプション引数
2.4のplotメソッドのオプション引数から "auto_open" がないのみで、他は同じ。

### 2.7 タイトルや軸ラベルを貼る

```python
import plotly.plotly as py
from plotly.graph_objs import *

data1 = Scatter(
    x = [1, 2, 3, 4, 5, 6, 7, 8, 9],
    y = [1, 2, 3, 4, 5, 6, 7, 8, 9],
    name = 'データ1'
)
data2 = Scatter(
    x = [8, 6, 4, 2],
    y = [16, 5, 11, 9],
    name = 'データ2'
)

data = Data([data1, data2])
layout = Layout(
    title = 'タイトル',
    xaxis = dict(
        title = 'x軸',
    ),
    yaxis = dict(
        title = 'y軸',
    )
)

fig = Figure(data=data, layout=layout)
py.plot(fig, filename='figure-name')
```

[f:id:Garfields:20171017230412p:plain]

## 3. オフラインで使ってみる

オフラインでは、アカウントの登録がいらず、手軽に実行することができる。  
オフラインでグラフをプロットするには、plotly.offline.plot() メソッドを使用ればよい。  
オフラインでプロットする場合には、plotメソッドのfilenameで指定されたファイル名でhtmlファイルが保存される。  

```python
import plotly
from plotly.graph_objs import *

data1 = Scatter(
    x = [1, 2, 3, 4, 5, 6, 7, 8, 9],
    y = [1, 2, 3, 4, 5, 6, 7, 8, 9],
    name = 'データ1'
)
data2 = Scatter(
    x = [8, 6, 4, 2],
    y = [16, 5, 11, 9],
    name = 'データ2'
)

data = Data([data1, data2])
plotly.offline.plot(data, filename='file-name')
```

### offline.plotメソッドのオプション引数

- show_link (default=True)  
Plotlyクラウドにエクスポートするためのリンクを右下に表示するかどうか  
- link_text (default='Export to plot.ly')  
エクスポートするためのリンクのテキスト  
- validate (default=True)  
グラフのキーが有効か検証する  
- output_type ('file' | 'div' - default 'file')  
  - 'file'  
  グラフはスタンドアローンのHTMLファイルとして保存され、Noneを返す  
  - 'div'  
  グラフとグラフを生成するスクリプトが含まれた HTML <div> を含んだ文字列を返す。グラフをHTMLファイルに埋め込みたい時に使用する。  
- include_plotlyjs (default=True)  
出力ファイルに plotly.js のソースコードを含める。すでに plotly.js のコードHTMLに含まれている場合は、Falseにする。  
- filename (default='temp-plot.html')  
出力されるファイル名。すでに同じ名前が存在する場合は、上書きされる。'output_type' が 'file' の時のみ有効である。  
- auto_open (default=True)  
ファイルを出力した後にwebブラウザで開くかどうか  
- image (default=None |'png' |'jpeg' |'svg' |'webp')  
ダウンロードされる画像のフォーマットの指定  
- image_filename (default='plot_image')  
ダウンロードされる画像の名前  
- image_height (default=600)  
画像の高さ (px)  
- image_width (default=800)  
画像の幅 (px)  

## 関連記事
http://python.zombie-hunting-club.com/index#Visualization

## 書籍

[asin:4873117488:detail]

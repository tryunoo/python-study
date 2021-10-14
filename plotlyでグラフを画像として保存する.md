## Index


[:contents]



## 1. はじめに
本稿では、plotlyで作成したグラフを画像形式で実行じに保存する方法を説明する。  
方法は、おそらく二種類あり、オンラインでの方法とオフラインでの方法がある。  

## 2. オンラインでの方法
オンラインでの方法では、plotlyのアカウント登録が必要になる。  
アカウントを登録して、オンラインで使用する方法は、以下記事を参照したい。  
[[Python] Plotly 入門](http://www.zombie-hunting-club.com/entry/2017/10/17/230636#2-オンラインで使ってみる)

オンラインで画像を取得するには、以下のように、save_asメソッドを使用する。  

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

py.image.save_as({'data':data}, filename='test', format='png')
```


## 3. オフラインでの方法
オフラインでの方法は、plotの引数に image を指定すれば良い。  
ただし、こちらの方法だと、ブラウザでhtmlファイルを自動オープンしなければならないので、GUIの環境でしか画像を保存できない。  

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

plotly.offline.plot(data, filename='test.html', image_filename='test', image='jpeg')
```

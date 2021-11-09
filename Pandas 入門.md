## Index


## 1. はじめに
Pandasを使用すると、データの作成、読み込み、解析を簡単に行うことができる。  
また、グラフも表示できるので便利である。  


## 2. データ作成
```python
# test.py
import pandas as pd
import numpy

dates = pd.date_range("2018/01/20", periods=10)
nums = numpy.random.randint(0, 100, (10, 2))
columns = ["既存顧客", "新規顧客"]

df = pd.DataFrame(nums, index=dates, columns=columns)

print(df)
```

```
% python3 test.py
            既存顧客  新規顧客
2018-01-20    93    99
2018-01-21    55    56
2018-01-22    87    84
2018-01-23    72     6
2018-01-24    25     0
2018-01-25    49    63
2018-01-26    94    15
2018-01-27     5    22
2018-01-28    53    10
2018-01-29     7    61
```

または、以下の書き方でも同様のことができる。
```python
import pandas as pd
import numpy

dates = pd.date_range("2018/01/20", periods=10)

df = pd.DataFrame({
        "既存顧客" : numpy.random.randint(0, 100, 10),
        "新規顧客" : numpy.random.randint(0, 100, 10)
        },
        index=dates
    )

print(df)
```


## 3. データ読み込み
Pandasは、csvやtsvなどからデータを読み込むことができる。  
今回は、以下の test.csv ファイルを読み込む。  

```
Date,既存顧客,新規顧客
2018-01-20,93,99
2018-01-21,55,56
2018-01-22,87,84
2018-01-23,72,6
2018-01-24,25,0
2018-01-25,49,63
2018-01-26,94,15
2018-01-27,5,22
2018-01-28,53,10
2018-01-29,7,61
```

```python
# test.py
import pandas as pd

df = pd.read_csv('test.csv', index_col='Date')

print(df)
```

```
% python3 test.py
            既存顧客  新規顧客
Date                  
2018-01-20    93    99
2018-01-21    55    56
2018-01-22    87    84
2018-01-23    72     6
2018-01-24    25     0
2018-01-25    49    63
2018-01-26    94    15
2018-01-27     5    22
2018-01-28    53    10
2018-01-29     7    61
```

## 4. データ取得
Pandas の DataFrame オブジェクトは、値を指定して柔軟に抽出することができる。

```python
print(df['新規顧客'])
```

```
Date
2018-01-20    99
2018-01-21    56
2018-01-22    84
2018-01-23     6
2018-01-24     0
2018-01-25    63
2018-01-26    15
2018-01-27    22
2018-01-28    10
2018-01-29    61
Name: 新規顧客, dtype: int64
```

```python
print(df[2:5])
```

```
既存顧客  新規顧客
Date                  
2018-01-22    87    84
2018-01-23    72     6
2018-01-24    25     0
```

```python
print(df['2018-01-22':'2018-01-24'])
```

```
既存顧客  新規顧客
Date                  
2018-01-22    87    84
2018-01-23    72     6
2018-01-24    25     0
```


## 5. データ操作
### 転置

```python
print(df.T)
```

```
% python3 test.py
Date  2018-01-20  2018-01-21  2018-01-22  2018-01-23  2018-01-24  2018-01-25  \
既存顧客          93          55          87          72          25          49   
新規顧客          99          56          84           6           0          63   

Date  2018-01-26  2018-01-27  2018-01-28  2018-01-29  
既存顧客          94           5          53           7  
新規顧客          15          22          10          61  
```

### 6. ソート

```python
print(df.sort_values(by='既存顧客'))
```

```
% python3 test.py
            既存顧客  新規顧客
Date                  
2018-01-27     5    22
2018-01-29     7    61
2018-01-24    25     0
2018-01-25    49    63
2018-01-28    53    10
2018-01-21    55    56
2018-01-23    72     6
2018-01-22    87    84
2018-01-20    93    99
2018-01-26    94    15
```

## 7. データプロット
プロットするには、matplotlib が必要になる。

```
% sudo pip3 install matplotlib
```

グラフの表示は、Jupyter Notebook 上で行う。
```
% jupyter notebook
```

また、自分の環境では、以下のコードを最初に実行しないと、グラフをプロットできなかった。
```
%matplotlib inline
```


```python
%matplotlib inline
import pandas as pd
import numpy

df = pd.read_csv('test.csv', index_col=0, encoding='utf-8')

df.plot()
```

[f:id:Garfields:20171021124137p:plain]

また、上記のコードでは日本語が正しく表示できていない。  
これを解消するには、以下のコードのようにフォントファミリーを指定すれば良い。  

```python
import pandas as pd
import numpy
import matplotlib

font = {'family' : 'Arial Unicode MS'}
matplotlib.rc('font', **font)

df = pd.read_csv('test.csv', index_col=0, encoding='utf-8')

df.plot(figsize=(11, 5))
```

[f:id:Garfields:20171021124145p:plain]

他にも、バーやヒストグラムなどのグラフをプロットできる。

```python
df.plot(kind='bar')
```

[f:id:Garfields:20171021124156p:plain]


## 参考
- [pandasのplotで日本語を使う](http://blog.mwsoft.jp/article/105958987.html)
- http://pandas.pydata.org/pandas-docs/version/0.21/

## 書籍

[asin:4873116554:detail]

[asin:1491957662:detail]

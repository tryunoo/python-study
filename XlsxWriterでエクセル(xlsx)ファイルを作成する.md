## Index


## 1. はじめに
[XlsxWriter](https://pypi.python.org/pypi/XlsxWriter)というPyhtonライブラリを使用すれば、Pythonで簡単にxlsxファイルを作成することができる。  

XlsxWriterは、xlsxファイルと100%の互換性があり、以下の操作が可能である。

- 各書式設定
- セルの結合
- 名前定義
- チャートの作成
- オートフィルタ作成
- データの入力規則設定
- ドロップダウンリスト作成
- 条件付き書式設定
- 画像挿入 (PNG/JPEG)
- 文字のデコレーション (文字色や大きさなど)
- セルのコメント
- テキストボックス操作
- Pandasとの統合
- 大容量ファイルへのメモリ最適化

今回は、XlsxWriterの基本的な使い方を説明する。

また、XlsxWriterは、pipでインストールすることができる。

```
% pip install xlsxwriter
```

## 2. ワークシートの作成
下記のコードを実行すると、sheet_name という名前のシートを持つ test.xlsx が作成される。  
add_worksheet の引数でシート名を指定しなかった場合、デフォルトでは、sheet1 になる。

```python
import xlsxwriter

workbook = xlsxwriter.Workbook('test.xlsx')
worksheet = workbook.add_worksheet('sheet_name')

workbook.close()
```

## 3. セルへの書き込み
セルへの値の書き込みは、以下のメソッドで行うことができる。

**worksheet.write(行, 列, 文字列/値/式)**
or
**worksheet.write(指定セルの文字列, 文字列/値/式)**

```python
import xlsxwriter

workbook = xlsxwriter.Workbook('test.xlsx')
worksheet = workbook.add_worksheet('sheet_name')

worksheet.write(0, 0, 3)
worksheet.write(1, 0, 5)
worksheet.write(0, 1, "A1 + A2")
worksheet.write('B2', "=SUM(A1:A2)")
worksheet.write('C1', "https://google.com")
worksheet.write('C2', False)
worksheet.write('C3', None)

workbook.close()
```

[f:id:Garfields:20171019211132p:plain]

また、式やURLをただの文字列として入力したい場合は、**write_strint()** メソッドを使用する。
```python
worksheet.write_string('B2', "=SUM(A1:A2)")
```

## 4. 書式を変更する
ここでは、以下を設定する。
- 文字の色
- 文字のスタイル
- セルの色

書式を設定するには、add_format() メソッドを使用する。  

```python
import xlsxwriter

workbook = xlsxwriter.Workbook('test.xlsx')
worksheet = workbook.add_worksheet()

fmt = workbook.add_format()
fmt.set_font_color('blue')
fmt.set_bold()
fmt.set_bg_color('yellow')

worksheet.write('B2', "文字だよー", fmt)

workbook.close()
```

[f:id:Garfields:20171019211146p:plain]

また、add_format() メソッドは、以下のような書き方もできる。
```python
fmt = workbook.add_format({
    'bold':True,
    'font_color':'blue',
    'bg_color':'#FFFF00',
})

worksheet.write('B2', "文字だよー", fmt)
```

その他のフォーマットは、[こちら](http://xlsxwriter.readthedocs.io/format.html)に詳細が記されているので、参照したい。

## 5. 条件付き書式の設定
XlsxWriterでは、条件付き書式も扱うことができる。  
以下は、値が50より下ならば背景を赤に、50以上ならば青にするプログラムである。

```python
import xlsxwriter
import numpy

workbook = xlsxwriter.Workbook('test.xlsx')
worksheet = workbook.add_worksheet('sheet_name')


fmt1 = workbook.add_format({'bg_color':'blue'})
fmt2 = workbook.add_format({'bg_color':'red'})

worksheet.conditional_format('A1:J10', {'type':     'cell',
                                        'criteria': '>=',
                                        'value':    50,
                                        'format':   fmt1})

worksheet.conditional_format('A1:J10', {'type':     'cell',
                                        'criteria': '<',
                                        'value':    50,
                                        'format':   fmt2})

values = numpy.random.randint(0, 100, (10, 10))

for x in range(10):
    for y in range(10):
        worksheet.write(x, y, values[x][y])

workbook.close()
```

[f:id:Garfields:20171019211204p:plain]

条件付き書式の詳細は、[こちら](http://xlsxwriter.readthedocs.io/working_with_conditional_formats.html)に記載されている。

## 参考
- http://xlsxwriter.readthedocs.io/

## 書籍
[asin:4873117380:detail]

[asin:4774142298:detail]

[asin:477415539X:detail]

[asin:B00XZTYMG6:detail]

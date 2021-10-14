## Index

## はじめに

## 受け渡し
renderメソッドの第三引数に辞書として受け渡すことでテンプレートで使用することができる。  
```python
from django.shortcuts import render

def index(request):
    data = "Hello World!"
    lis = ["A1", "B2", "C3", "D4"]
    dic = {"Name":"John", "Age":36}

    return render(request, "index.html", {'data':data, "lis":lis, "dic":dic})
```

## 変数の表示
基本形は以下になる。  
```python
{{ Variable }}
```

また、リストや辞書は、以下のようにキーを [] ではなく . で指定する。  
```python
{{ list.0 }}

{{ dict.key }}
```

## CSRFトークン
```python
{% csrf_token %}
```
CSRF対策  
フォームでPOSTするときに必要  
以下のように埋め込む  
```html
<form method="post">
  {% csrf_token %}
  <input type="submit">
</form>
```


## コメント
```
{# Comment #}
```


## if文
```python
{% if a=1 %}
  ...
{% elif a=2 %}
  ...
{% else %}
  ...
{% endif %}
```

## for文
```python
{% for item in items %}
  <li>{{ item }}</li>
{% endfor %}
```

### 辞書のfor
```python
{% for key, value in dic.key %}
  <li>{{ key }} : {{ value }}</li>
{% endfor %}
```

```html
<li>C : 3</li>
<li>D : 4</li>
<li>A : 1</li>
<li>B : 2</li>
```

### 使える変数
例えば、以下のように forloop.counter は、ループ回数を保持している。  
```html
{% for item in items %}
  <li>{{ forloop.counter }}</li>
{% endfor %}
```

```html
<li>1</li>
<li>2</li>
<li>3</li>
<li>4</li>
<li>5</li>
```

他にも、以下の変数がある。  

|変数                 |説明                   |
|-------------------|---------------------|
|forloop.counter    |現在のループ回数（1からカウント）    |
|forloop.counter0   |現在のループ回数（0からカウント）    |
|forloop.revcounter |末尾から数えたループ回数（1からカウント）|
|forloop.revcounter0|末尾から数えたループ回数（0からカウント）|
|forloop.first      |最初のループならばTrue        |
|forloop.last       |最後のループならばTrue        |
|forloop.parentloop |入れ子のループの場合、一つ上のループを表す|


## cycle文
```
data = [1,2,3,4,5,6]
```

```python
{% for x in data %}
  <li>{{ x }} : {% cycle 'odd' 'even' %}</li>
{% endfor %}
```

出力は以下になる。  

```html
<li>1 : odd</li>
<li>2 : even</li>
<li>3 : odd</li>
<li>4 : even</li>
<li>5 : odd</li>
<li>6 : even</li>
```

## フィルター
djangoテンプレートには、フィルターという機能がある。  
例えば、title フィルターは、英単語の先頭を大文字にするものである。  
```python
data = "this is a sample text!"
```

```
{{ data|title }}
```
出力以下になる。  
```
This Is A Sample Text!
```

他にも、文字列の長さを返すものや、日付を返すものなど多くのフィルターがあるので、公式リファレンスを参照したい。  

https://docs.djangoproject.com/en/1.11/ref/templates/builtins/#built-in-filter-reference

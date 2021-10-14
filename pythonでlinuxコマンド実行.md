
### コマンド実行
```python
import subprocess

res = subprocess.call("ls")
print res # 成功すれば0が返る
```
### コマンドを実行し、出力を取得
```python
import subprocess

res = subprocess.check_output("ls")
print res # "ls"の実行結果
```

### コマンドを引数付きで実行する
```python
import subprocess

command = ["ls", "-l", "-a"]
res = subprocess.check_output("ls")
print res # "ls -la"の実行結果
```

### subprocess.run
Python3.5以上で使用できる。
戻り値、実行結果を取得できる。

##### 自動出力する場合
```python
>>> import subprocess
>>> res = subprocess.run(["ls", "-l"])
total 4
drwxrwxr-x 2 r00t r00t 4096  8月  3 10:10 test
-rw-rw-r-- 1 r00t r00t    0  8月  3 10:10 test.txt
>>> res
CompletedProcess(args=['ls', '-l'], returncode=0)
```

##### 出力を取得する場合
```python
>>> res = subprocess.run(["ls", "-l"], stdout=subprocess.PIPE)
>>> res
CompletedProcess(args=['ls', '-l'], returncode=0, stdout=b'total 4\ndrwxrwxr-x 2 r00t r00t 4096  8\xe6\x9c\x88  3 10:10 test\n-rw-rw-r-- 1 r00t r00t    0  8\xe6\x9c\x88  3 10:10 test.txt\n')
```


```python
import requests

s = requests.Session()

r = s.get('http://httpbin.org/cookies/set/foo/bar')

print(s.cookies.get_dict())
# {'foo': 'bar'}
```

```python
import requests

cookies = {"cookie1":"test1", "cookie2":"test2"}

s = requests.Session()

r = s.get('http://httpbin.org/cookies', cookies=cookies)

print(r.text)
# {
#   "cookies": {
#     "cookie1": "test1",
#     "cookie2": "test2"
#   }
# }
```



```
% sudo pip3 install tqdm
```

```python
from tqdm import tqdm
import time

for x in tqdm(range(10)):
    time.sleep(1)
```

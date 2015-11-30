# Python
## 無限に入れ子になってるdefaultdict
```python
# from http://d.hatena.ne.jp/cheeseshop/20091226
from collections import defaultdict

class recursive_defaultdict(defaultdict):
    def __init__(self):
        self.default_factory = type(self)
```

# Python tips
## 無限に入れ子になってるdefaultdict
```python
# from http://d.hatena.ne.jp/cheeseshop/20091226
from collections import defaultdict

class recursive_defaultdict(defaultdict):
    def __init__(self):
        self.default_factory = type(self)
```
## 簡単な単語ID辞書
```python
# from https://twitter.com/neubig/status/328679465137872896
from collections import defaultdict

word_ids = defaultdict(lambda: len(word_ids))
```
単語を単語IDにマッピングする辞書

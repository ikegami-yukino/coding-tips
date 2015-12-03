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

## 文字列hashingの比較
```python
import hmac
%timeit int(hmac.new("ファンタジスタドール").hexdigest(), 16)
#=> 10000 loops, best of 3: 14.3 µs per loop

import hashlib
%timeit int(hashlib.sha256("ファンタジスタドール").hexdigest(), 16)
#=> 100000 loops, best of 3: 2.45 µs per loop

%timeit int(hashlib.md5("ファンタジスタドール").hexdigest(), 16)
#=> 100000 loops, best of 3: 2.06 µs per loop

import mmh3
%timeit mmh3.hash("ファンタジスタドール")
#=> 1000000 loops, best of 3: 183 ns per loop

%timeit hash("ファンタジスタドール")
#=> 10000000 loops, best of 3: 62.6 ns per loop
```

- builtinの`hash`関数は最速だが、永続性が必要なものには向いてない (`PYTHONHASHSEED`環境変数を設定したPython3以外の環境では値が変わってしまうことがある)
- `mmh3`も早いがインストールが必要
- `hashlib.md5`はインストール不要で値が固定なものの中では一番早い

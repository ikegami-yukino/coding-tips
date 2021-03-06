# Python tips
## listの順番を保持しながら重複を除去
```python
>>> x = [3, 2, 1, 1]
>>> sorted(set(x), key=x.index)
[3, 2, 1]
```

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

## メモリ使用量
```python
import subprocess
import os

subprocess.check_output(["ps", "up", str(os.getpid())]).decode('utf8').split()[16]
```

## 文字列hashingの比較
```python
import hmac
%timeit int(hmac.new("ファンタジスタドール").hexdigest(), 16)
#=> 10000 loops, best of 3: 14.3 µs per loop

import hashlib
%timeit int(hashlib.sha512("ファンタジスタドール").hexdigest(), 16)
#=> 100000 loops, best of 3: 2.77 µs per loop

%timeit int(hashlib.sha384("ファンタジスタドール").hexdigest(), 16)
#=> 100000 loops, best of 3: 2.67 µs per loop

%timeit int(hashlib.sha256("ファンタジスタドール").hexdigest(), 16)
#=> 100000 loops, best of 3: 2.45 µs per loop

%timeit int(hashlib.sha224("ファンタジスタドール").hexdigest(), 16)
#=> 100000 loops, best of 3: 2.36 µs per loop

%timeit int(hashlib.sha1("ファンタジスタドール").hexdigest(), 16)
#=> 100000 loops, best of 3: 2.08 µs per loop

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

## 文字種ごとのリスト
```python
import string

HIRAGANA = [chr(i) for i in range(12353, 12353+86)]  # ぁ-ゖ
KATAKANA = [chr(i) for i in range(12449, 12449+90)]  # ァ-ヺ
KANJI = [chr(i) for i in range(19968, 19968+20935)]  # 一-鿆

list(string.ascii_lowercase)  # abcdefghijklmnopqrstuvwxyz
list(string.ascii_uppercase)  # ABCDEFGHIJKLMNOPQRSTUVWXYZ
list(string.ascii_letters)  # string.ascii_lowercase + string.ascii_uppercase (i.e.) abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ

list(string.digits)  # 0123456789
KANJI_OLD_DIGITS = ["零", "壱", "弐", "参", "肆", "伍", "陸", "漆", "捌", "玖"]

list(string.octdigits)  # Characters used in octal representation: 01234567
list(string.hexdigits)  # Characters used in hex representation: 0123456789abcdefABCDEF

list(string.punctuation)  # !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~

list(string.whitespace)  # space + TAB + line endings (Mac: ¥r, Unix: ¥n, and DOS: ¥r¥n)

list(string.printable)  # string.ascii_letters + string.ascii_letters + string.punctuation + string.whitespace
PRINTABLE_ASCII_WITHOUT_TAB_AND_LINE_ENDINGS = list(string.digits + string.ascii_letters + string.punctuation + " ")
ALL_ASCII = [chr(i) for i in range(128)]  # [NUL]-[DEL]
```

## HTML Entityを元の文字に戻す

```python
import html
s = '&lt;foo&gt;bar&lt;/foo&gt;'
html.unescape(s)
#=>'<foo>bar</foo>'
```

## n-gram

```python
def to_ngrams(item, max_n):
    return [to_ngram(item, n) for n in range(2, max_n + 1)]

def to_ngram(item, n):
    return [item[i:i+n] for i in range(len(item)-n+1)]
```

## 文字列の類似度

```python
import difflib
a = u”スパゲッティ”
b = u”スパゲティー”
print(difflib.SequenceMatcher(None, a, b).ratio())
```
from http://tt-house.com/2010/04/python-sequencematcher.html

## パイプからの標準入力を受け取る

```python
import sys

for x in sys.stdin:
    sys.stdout.write(x)  # とりあえず確認用にパイプから受け取った文字列を標準出力してみる
```

## 2ちゃんねるまとめサイト検出用正規表現

2014/01/08(水) 12:30:37.67 ID:+pyxCrmX0みたいなやつにマッチする

```python
import re
re_2ch_post = re.compile(u'[^2]2[0-9]{3}/\d{2}/\d{2}\(.\) \d{2}:\d{2}:\d{2}\.\d{2} ID:[\w\d\+\/]+')
```

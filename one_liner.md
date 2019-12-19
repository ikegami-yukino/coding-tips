# One-liner

## テキスト処理
### ディレクトリ以下のファイルを結合
```sh
find /tmp/text -name "*.txt" -print0 | xargs -0 -I % cat %  >> /tmp/hoge.txt
```

### 再帰的に文字コード変換
```sh
find . -type f -exec sh -c "iconv -f eucjp -t UTF-8 {} > {}.utf8"  \; -exec mv "{}".utf8 "{}" \;
```
### 置換
```perl
perl -ne 's/foo//g; print' < foobar.txt
```

### TSVをCSVに変換
```sh
cat example.tsv | tr "\t" "," > example.csv
```

### 空行の削除
```perl
perl -i -ne '/^\s*$/ or print' foobar.txt
```

### 指定行を削除
```perl
perl -ne '$i++; print if ($i != 1)' < foobar.txt
```

### 重複行の削除
```perl
perl -ne 'print if !$a{$_}++' < foobar.txt
```

### word count
```sh
cat hoge.txt | mecab | grep 名詞 | awk '{print $1}' | sort | uniq -c| sort -n -r
```

### 全角英数字と半角カタカナを半角英数字と全角カタカナにする
```sh
nkf -m0Z1 -W -w txt
```
### sortコマンドでタブ区切りのファイルを指定する
以下のようにしてタブ区切りで2番目のキーでソートすることができます．
```sh
sort -t $'\t' -k 2,2 < input.txt
```
from https://web.archive.org/web/20120419132910/http://d.hatena.ne.jp/nokuno/20120121/1327139192

### 1000行おきにデータをサンプリングする
```
perl -ne '$i++; print unless ($i % 1000)' < input.txt
```
from https://web.archive.org/web/20120419132910/http://d.hatena.ne.jp/nokuno/20120121/1327139192

### タブ区切りのフィールドを指定して取り出す
```
$ cut -f 3 < input.txt
$ cut -f 3,4 < input.xt
```
ただし，フィールドを入れ替えることはできないので，その場合はawkを使うことになります．
```
$ awk '{print $3, $2;}'
```
awkだとデフォルトがスペース区切りなので，入力をタブ区切りにするにはsortと同じように$'\t'を区切り文字に指定し，出力をタブ区切りにするにはOFS="\t"を指定する必要があります．ちょっと面倒ですが…
```
$ awk -F $'\t' '{OFS="\t"; print $3, $2;}'
```
cutコマンドの逆で行数が同じ複数のファイルを列結合（SQLでいうJOIN）するには，pasteコマンドが使えます．

from https://web.archive.org/web/20120419132910/http://d.hatena.ne.jp/nokuno/20120121/1327139192

### 漢字混じりテキストの読みの文字数をカウント

```
echo 漢字混じりテキスト | mecab -d `mecab-config --dicdir`/mecab-ipadic-neologd -O yomi | wc -m
```

## ハードウェア情報
### Mac
```
system_profiler SPHardwareDataType
```
### Ubuntu
#### CPU
```
sudo lshw -short -class processor
```
#### 最大Memory
```
cat /proc/meminfo | grep MemTotal
```
### HDD/SSDの使用率
```
df -h
```
### GPUメモリの使用率
```
nvidia-smi -l
```
## Python ライブラリの情報
### numpy
```
python -c "import numpy as np; np.show_config()"
```
### TensorFlow
```
python -c "from tensorflow.python.client import device_lib; print(device_lib.list_local_devices())"
```
### Chainer
```
python -c "import chainer; chainer.print_runtime_info()"
```

## その他
### コマンドを時刻付きで1行ずつ出力&保存
```sh
何かコマンド | awk '{ print strftime("[%Y/%m/%d %H:%M:%S]"), $0 } { fflush() }'|tee 保存先
```
### MeCab辞書のソート
```sh
LC_ALL=C ls *.csv |xargs -I{} sort -o {} -u -n -k 2 -t , {}
```

## Link
- [日本語の自然言語処理には Perl も便利 - アスペ日記](http://d.hatena.ne.jp/takeda25/20110823/1314105549)
- [[O] テキストファイルをソートするときに頻繁に使うUnixコマンド](http://diary.overlasting.net/2012-01-21-1.html)

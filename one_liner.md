#One-liner

## テキスト処理
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

## その他
### コマンドを時刻付きで1行ずつ出力&保存
```sh
何かコマンド | awk '{ print strftime("[%Y/%m/%d %H:%M:%S]"), $0 } { fflush() }'|tee 保存先
```



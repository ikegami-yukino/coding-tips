# Vim

## 正規表現
### 空行を除去
```vim
:%s/\n$//g
```

### URLを除去
```vim
:%s/https\?:\/\/[a-z\.A-Z0-9\/\-_:&\?#=]\{2,\}//g
```

### 行ごと削除
```vim
:g/hoge/d
```

## 複数ファイルを一括置換
from http://zx.jpn.org/b/20081025/155/vim/vim-mluti-file-replace

例えば、cのソースコードファイルだけを対象にする場合は
```vim
:args *.c
```
フォルダー内も対象とるする場合は
```vim
:args **/*.c
```
対象となるファイルを確認するには
```vim
:args
```
そんでもって変換は
```vim
:argdo %s/hoge/fuga/g | update
```


## その他
### 重複除去&ソート
```vim
:sort u
```

### nkfで全角英数字を半角に
```vim
:%!nkf -m0Z1 -W -w
```

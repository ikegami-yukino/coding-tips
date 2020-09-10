# Vim

## 正規表現
### Very Magicを使う
マッチするパターンの前に `\v` を付けるだけで、グルーピングを `\(\)` と書かずに `()` と書けば通じるようなったりして便利。

```vim
:%s/\v<pattern>//g
```
### 検索した後は置換する時にパターンを書かなくていい
```vim
/abcd
:%s//ABCD/g " Replace abcd with ABCD
```

### 置換する際にある文字列を検索に含めたいけど置換対象には含めたくない場合
`\zs` は置換対象文字列の始まりの指定、 `\ze` は置換対象文字列の終わりの指定

```vim
:%s/\zsあいうえお\ze.*検索に含める文字列/アイウエオ/g
```vim

### 空行を除去
```vim
:%s/\n$//g
```

### パターンにマッチした数を表示する
```vim
:%s/<pattern>//gn
```

### URLを除去
```vim
:%s/https\?:\/\/[a-z\.A-Z0-9\/\-_:&\?#=]\{2,\}//g
```

### 行ごと削除
```vim
:g/hoge/d
```

### HTMLタグ除去
```vim
:%s/<.\{-}>//g
```

## 複数ファイルを一括置換
from http://zx.jpn.org/b/20081025/155/vim/vim-mluti-file-replace

例えば、cのソースコードファイルだけを対象にする場合は
```vim
:args *.c
```
フォルダー内も対象とする場合は
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
### インデントの整形
```vim
gg=G
```
### 一括コメントアウト
```vim
行頭を範囲選択して I#<ESC>
```

### 重複除去&ソート
```vim
:sort u
```

### 文字数カウント
```vim
g Ctrl-g
```

### nkfで全角英数字を半角に
```vim
:%!nkf -m0Z1 -W -w
```

### 大文字/小文字変換

ヴィジュアルモードで `u` で小文字、 `U` で大文字になる。

### :set expandtab設定時にタブ文字を挿入

Control-v Tab

### アメブロ本文抽出
```vim
:%s/\_.*<!-- google_ad_section_start -->//g
:%s/<!-- google_ad_section_end -->\_.*//g
```

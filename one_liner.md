#One-liner

## コマンドを時刻付きで1行ずつ出力&保存
```sh
何かコマンド | awk '{ print strftime("[%Y/%m/%d %H:%M:%S]"), $0 } { fflush() }'|tee 保存先
```
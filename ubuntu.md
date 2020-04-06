# Ubuntu

## Ubuntuのバージョンとかを表示
```bash
cat /etc/os-release
```

## セキュリティアップデートだけする
```
sudo unattended-upgrades
```

## コマンドのバージョンの切り替え
```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 2
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.4 1
```

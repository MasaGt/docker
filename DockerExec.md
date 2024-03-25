### execコマンド

- 実行中のコンテナ内でコマンドを実行するためのコマンド

例
```bash
docker run -d ubuntu
# コンテナ内でlsを実行させる
docker exec <コンテナ名 or ID> ls
# bin
# boot
# dev
# etc
# home
# lib
# media
# mnt
# opt
# proc
# root
# run
# sbin
# srv
# sys
# tmp
# usr
# var
```

### execでコンテナ内に入る

- docker exec -it <コンテナ名 or ID> bash

    指定したコンテナ内でbashコマンドを実行している

    そもそもbashと打つと、新しいプロセスでbash(非ログイン)シェルが立ち上がる

**[-it オプションについてはこちら参照](./itオプション.md)**
### docker attach　ってなに  

- コンテナに接続 や コンテナ内に入る ようなイメージ

- 実態は、コンテナ内のプロセス(PID=1のプロセス)に、ホストの標準入出力(キーボードとディスプレイ)を接続する

例

```bash
docker run -dit ubuntu
docker attach <コンテナ名 or ID>
# root@18e48f607b54:/# (プロンプトがlinuxのものになる = ubuntuのコンテナに接続した)

ps -A
# PID TTY          TIME CMD
#     1 pts/0    00:00:00 bash
#    10 pts/0    00:00:00 ps
```

### docker exec との違い
-　attach  
    - コンテナのPID1を使う  
    - コンテナからexitした後、コンテナは起動終了する

- exec 
    - コンテナ内で新しく/bin/bashプロセスを立ち上げる  
    - コンテナからexitした後もコンテナは起動している

### atachとexecどっちを使うべきか
基本的にはexec  
- 理由
    1. exitした後コンテナが終了するとまずいケースが多い(実運用において)

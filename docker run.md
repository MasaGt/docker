### 概要
概要は「基本コマンド.md」を参照

### 主なオプション
- --name:　作成/起動するコンテナの名前を指定する
- -p: 作成/起動するコンテナを動かすポート番号の指定
- -d: 作成/起動するコンテナをバックグラウンドで動かす
- -v: ボリュームをマウントする

- -e: 環境変数を設定し、渡す

```bash
docker run --name sample -d httpd

docker run --name db -e MYSQL_ROOT_PASSWORD=root mysql
```

webサーバーのコンテナをダウンロード/"sample"という名前でコンテナ作成/起動する

**このコンテナを停止する場合は docker cconatiner stop sample**
**もしくはコンテナ名の代わりにコンテナIDを指定する**

---

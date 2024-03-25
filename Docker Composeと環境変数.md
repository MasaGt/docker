### Docker Composeで環境変数を設定する方法
いくつか方法がある

1. compose.ymlの中でenvironmentに環境変数を宣言する 

2. shell上の環境変数を使う

3. .envファイルで環境変数を宣言する

*3は docker run コマンドの [--env-file オプション](./ENV-FILE.md)と同じ

---

### compose.ymlの中でenvironmentに環境変数を宣言する

```yml
services:
  <コンテナ名>:
    image: <イメージ名>:<バージョン>
    environment:
      - <キー名>: <値>
      - <キー名>: <値>
      #以下の書き方でも良い
      - <キー名>=<値>
```
*ymlファイルにベタ書きするので、重要な情報を書きにくい

---

### shell上の環境変数を使う

```bash
export IMAGE=httpd:latest
```

<br>

```yml
services:
  <コンテナ名>:
    image: ${IMAGE}
```

---

### .envファイルで環境変数を宣言する

```
<!-- .envにて環境変数の設定 -->
ROOTPASS=root
DB=db
USER=user
PASS=user
```

<br>

```yml
services:
  image: mysql:latest
  environment:
   MYSQL_ROOT_PASSWORD=${ROOTPASS}
   MYSQL_DATABASE=${DB}
   MYSQL_USER=${USER}
   MYSQL_PASSWORD=${PASS}
```

<br>

```bash
docker compose up -d
```

*この時、compose.ymlの親ディレクトリ内の.envファイルを自動的に探しにいく

*.env ファイルで定義されたものよりも、シェル環境で設定された値が優先される

*.env ファイルによる機能は docker-compose up コマンドを使うときだけ動作する
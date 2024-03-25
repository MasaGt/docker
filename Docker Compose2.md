#### Docker Composeの使い方
1. Docker Composeをインストールする  
  (Docker ComposeはPythonで書かれたプログラムなので。Pythonの実行環境もダウンロードする)

  *Windows/Mac版のDocker Desktopには最初からDocker Composeが入っているので、ここはスキップしていい*


2. YAML(定義)ファイルを置くフォルダを作成する

3. もし必要であれば、その他ファイル等も同一フォルダに置く

4. 定義ファイルの名前をcompose.yml(compose.yaml)にする  
\*ファイル名は**docker-compose.yml**でもいいが、docker-composeのバージョンが2以上では、compose.ymlが優先される  
[参考サイト](https://zenn.dev/miroha/articles/whats-docker-compose-v2)

5. composeコマンドを実行する(コンテナ作成)

```bash
# [定義ファイルがカレントディレクトリに存在する場合]
# Docker Compose V1
docker-compose up [引数]
# Docker Compose V2から
docker compose up [オプション]

# 独自の定義ファイル名をつけた or
# 定義ファイルがその他の場所にある場合

# Docker Compose V1
docker-compose -f [定義ファイルのパス] [その他のオプション] up [引数]

# Docker Compose V2から
docker　compose -f [定義ファイルのパス] [その他のオプション] up [引数]
```

*代表的なオプション  
-d: バックグラウンドで実行する

*docker-compose コマンドは docker compose　コマンドに置き換えられていく  

[互換性に関する参考サイト](https://docs.docker.jp/compose/cli-command-compatibility.html)

[docker composeコマンドの使い方](https://matsuand.github.io/docs.docker.jp.onthefly/engine/reference/commandline/compose/)

---

#### 定義ファイルの書き方

[定義ファイルサンプルを参照](./docker-compose.yml)


---

#### "サービス"とは

Docker Composeではコンテナの集合体のことをサービスと呼ぶ

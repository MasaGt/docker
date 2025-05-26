### Dockerfileとは
- Docker Image を作るための元ネタ
- Docker Image の定義ファイルで、Docker File をビルドすることでDocker Image が作成される
- 拡張子は無いただのテキストファイル
- \#でコメント

---

#### Dockerfileの記述例

```
FROM イメージ名:バージョン
(COPY コピー元パス コピー先パス)
(ADD コピー元パス コピー先パス)
(その他のコマンドもある)
```

*FROM でイメージのバージョンを指定しない場合は latest が暗黙的に指定されたことになる  
*コピー元パス(Dockerfile 内のファイルからの相対パス)  
*コピー先パス(コンテナ内の絶対パス)  

---

#### Dockerfile で使う命令文

- #### COPY, ADD

    - ローカルのファイルやディレクトリをイメージにコピーするための命令

    - 詳しくは[こちら](./Dockerfile-COPY-ADD.md)を参照

<br>

- #### RUN, CMD, ENTRYPOINT

    - コマンドを実行するための命令

    - 詳しくは[こちら](./Dockerfile-RUN-CMD-ENTRYPOINT.md)を参照

<br>

- #### WORKDIR

    - Dockerfile での命令実行時 (RUN、CMD、ENTRYPOINT、COPY、ADD 等) の作業ディレクトリ（working directory）を指定する命令

    - また、コンテナ起動時のデフォルトのディレクトリも、Dockerfile 内で最後に指定された WORKDIR になる

    - 環境変数の利用も可能

    - Dockerfile 内で複数回の利用が可能

    - ★現在のディレクトリに存在しないパスを指定すると、現在のディレクトリに WORKDIR で指定したディレクトリを作成する

    - 1回の WORKDIR 命令でイメージに1つのレイヤーを追加する

        <img src="./img/Dockerfile-WORKDIR_1.svg" />

<br>

- #### USER

    - Docker イメージ内で実行されるプロセス (RUN, CMD, ENTRYPOINT の命令等) がどのユーザーとして実行されるかを指定するための命令

    - また、コンテナ起動時のデフォルトユーザーも、Dockerfile 内で最後に指定された USER になる

    - Dockerfile内で複数回の利用が可能

    - ユーザー名でも UID でも指定可能

    - 1回の USER 命令でイメージに1つのレイヤーを追加する

        <img src="./img/Dockerfile-USER_1.svg" />

<br>

- #### ENV

    - 環境変数をセットするための命令

        - key と value のセットで指定する

    - Dockerfile 内で複数回の利用が可能

    - 1回の ENV 命令で複数の環境変数をセットできる (key=value の形式のみ)

    - 1回の ENV 命令でイメージに1つのレイヤーを追加する

    - ★ENV でセットした環境変数を Dockerfile 内で参照したければ **shell 形式 or シェルを起動した exec 形式 (/bin/sh -c)** で参照する必要がある

        <img src="./img/Dockerfile-ENV_1.svg" />

    <br>

    ##### ENV の疑問

    - Q: ENV で環境変数をセットする際、その環境変数はその時の USER だけの環境変数なのか?

    - A: ENV で設定された変数は、以降のすべてのレイヤー（命令）で有効。また、ENV で設定された環境変数は、コンテナが起動されたときに使われるデフォルトの環境変数になる

        <img src="./img/Dockerfile-ENV_2.svg" />

<br>
<br>

参考サイト

[Dockerfileでよく使う命令一覧【FROM,USER,WORKDIR,COPY,RUN】](https://docker.hatenablog.jp/entry/2023/05/10/190414)

[Dockerfile リファレンス](https://docs.docker.jp/v1.11/engine/reference/builder.html)

---

### 実際に Docker File からイメージを作成してみる

```bash
# 下準備
touch Dockerfile

# ローカルからコンテナへコピーするファイルの準備
touch copy.txt
```

<br>

```
# Dockerfileの中身

FROM ubuntu

WORKDIR /home

RUN echo "Hello World" >> sample.txt

COPY copy.txt copy.txt

WORKDIR /
```

<br>

- `docker buid` コマンドで dockerfile からイメージをビルドする

    - `-t (--tag)` オプションでイメージに名前とタグを付与することができる

        ```docker
        # 名前:タグの形で指定する
        # タグを省略するとlatestになる

        # イメージ名「test」、タグ「1.0」でイメージをビルドする例
        docker buid -t test:1.0
        ```

    <br>

    - `-f (--file)` オプションで Dockerfile のファイル名(パスも)を指定することができる

        ```docker
        # 現在のディレクトリからtest/myFileをDockerFileとして実行する例
        docker build -f .test/myFile
        ```

<br>

```bash
# Dockerfileからイメージ作成
# 現在のディレクトリにあるDockerFileからtestという名前のDocker Imageを作成する
docker build -t test .

# testイメージがあることを確認
docker image ls

# testイメージからコンテナ作成
docker run --name test -dit test

# コンテナ内に入る
docker exec -it test bash

# コンテナ内での現在のディレクトリを確認
ls -la

# /homeにsample.txtとcopy.txtがあることを確認
ls /home
```

### save コマンドとは

- 1つまたは複数のイメージを tar アーカイブに保存（デフォルトでは標準出力にストリーミング）するためのコマンド

    ```docker
    docker save [OPTIONS] IMAGE [IMAGE...]
    ```

<br>

- 以下は、Docker イメージを test.tar として出力する例

    <img src="./img/Docker-Save_1.svg" />

<br>
<br>

参考サイト

(docker save)[https://docs.docker.jp/engine/reference/commandline/save.html]

---

### -o オプション

- 出力先のファイルを指定する

- ★指定しない場合、save コマンドの結果は標準出力に出力される

    - 標準出力への結果を `>` でリダイレクトすることも可能 = -o オプションで出力先を指定するのと同じ

        <img src="./img/Docker-Save_2.svg" />

---

### docker save で出力したイメージの形式

- docker-archive 形式で出力される

    - -o オプションで出力しても、標準出力への結果をリダイレクトしても docker-archieve 形式

        <img src="./img/Docker-Save_3.svg" />
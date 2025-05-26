### COPY

- ローカルのファイルやディレクトリをイメージにコピーするための命令


---

### ADD

- 基本的には [COPY](#copy) と同じく、ファイルをイメージにコピーするための命令

- ローカルの圧縮ファイルADDの対象にすると、ADD先のイメージに**解凍してコピーする**

    ```docker
    # my-archive.tar.gz解凍されてから、イメージ内 の/usr/src にコピーされる

    ADD my-archive.tar.gz /usr/src/
    ```

<br>

- ADD対象にURLを指定すると、そのURLからADD先のイメージに**ダウンロードしてコピーする**

    - ★悪意のあるコードやコンテンツがプロセスに取り込まれる可能性があることに注意する必要がある

    ```docker
    # https://example.com/file.tar.gz をダウンロードしてイメージ内の /usr/src にコピーする

    # ★しかも、圧縮ファイルなので、解凍されてからコピーする

    ADD https://example.com/file.tar.gz /usr/src
    ```

---

### ADDとCOPYコマンドの違い

- COPY はソース側のマシンにあるものをイメージへコピーするだけというシンプルな機能であるのに対し、 ADD は対象が圧縮ファイルであればそれを展開してイメージへ移し、リモート上のリソースも引っ張ってくる機能がついている

<br>
<br>

参考サイト

[Dockerfile の ADD と COPY の違いを結論から書く](https://qiita.com/YumaInaura/items/1647e509f83462a37494)

[Docker のベストプラクティス: Dockerfile の ADD 命令と COPY 命令の違いを理解する](https://www.docker.com/ja-jp/blog/docker-best-practices-understanding-the-differences-between-add-and-copy-instructions-in-dockerfiles/)

[DockerfileにてなぜADDよりCOPYが望ましいのか](https://qiita.com/momotaro98/items/bf34eef176cc2bdb6892)

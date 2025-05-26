### RUN

- Docker イメージ内でコマンドを実行するための命令

    ```docker
    RUN <command>

    # apt update を実行する例
    RUN apt update
    ```

<br>

- ★イメージ作成時（= docker build 時）に RUN で指定したコマンドが実行される

    - ソフトウェアのパッケージインストールなど、**最初に Dcoker イメージに仕込んでおきたい設定を行っておく際に利用される**

<br>

- ★1回の RUN で1つレイヤーが追加される

    - レイヤー数を増やしたくない場合は、[パイプ (|) などでコマンドをつなげる](#や--で複数コマンドをつなげる方法)必要がある

        <img src="./img/Dockerfile-RUN_1.svg" />

<br>

- 2つの記法がある

    1. **shell 形式**

        - シェルによってコマンドが解釈されて実行される記法

        - ★この記法によって実行されるコマンドの前に `/bin/sh -c` が付与される = (sh シェルによってコマンドが実行される)

        ```docker
        RUN <command>

        # apt update を実行する例
        RUN apt update
        ```

    <br>

    2. **exec 形式**

        - シェルに解釈されずに、コマンドがそのまま実行される記法

            ```docker
            RUN ["<command>", "<arg1>", "<arg2>"...]

            # apt update を実行する例
            RUN ["apt", "update"]
            ```

        <br>

        - `/bin/bash -c` を exec 形式で実行することで、実質 shell 形式と同じ結果になる

            ```docker
            RUN ["/bin/sh", "-c", "apt update"]
            ```

    <br>

    ##### shell 形式と exec 形式の違い

    - shell 形式では、コマンドがシェルの環境変数やエイリアスなどの影響を受ける + シェルの機能であるパイプなどが利用できる

    - 一方、exec 形式では、コマンドはシェルの環境変数やエイリアスなどの影響を受けない + パイプなどのシェルの機能が使えない

    <img src="./img/Docker-shell-exec-Difference_1.svg" />

<br>
<br>

参考サイト

shell形式の /bin/sh の意味について: [知っておきたいシェルスクリプトの基礎知識 その1](https://gihyo.jp/admin/serial/01/unix/0020)

[DockerfileのCMDにおけるshell形式とexec形式の違い](https://zenn.dev/hisamitsu/articles/8388214d4ab0f3)

[DockerfileのCMDの記述の仕方](https://zenn.dev/agent_grow/articles/d4e5f0a2d85008)

---

### CMD

- [RUN](#run) と同様にコマンドを実行するための命令

    ```docker
    CMD <command>

    # JavaScriptファイル main.js を実行する例 (Node.jsはインストール済みであること)
    CMD node main.js
    ```

<br>

- RUN と同様に shell 形式と exec 形式の2つの書き方がある

<br>

- ★コンテナの起動時に CMD に指定したコマンドが実行される

    - = docker run と docker start の時に実行される

<br>

- ★★Dockerfile 内で1回しか実行されない

    - CMD 命令が複数箇所記述されていても、**実行されるのは最後の1箇所のみ**

        <img src="./img/Dockerfile-Multi-CMD_1.svg" />

<br>

- ★1回の CMD で1つレイヤーが追加される

---

### ENTRYPOINT

- [RUN](#run) と同様にコマンドを実行するための命令

<br>

- RUN と同様に shell 形式と exec 形式の2つの書き方がある

<br>

- [CMD](#cmd) と同様に、コンテナの起動時に ENTRYPOINT に指定したコマンドが実行される

<br>

- CMD と同様に ENTRYPOINT は Dockerfile 内で1回しか実行されない

    <img src="./img/Dockerfile-Multi-ENTRYPOINT_1.svg" />

<br>

- CMD と同様に1回の ENTRYPOINT で1つレイヤーが追加される

---

### RUN, CMD, ENRTYPOINTの違い

- 指定された**コマンドが実行されるタイミング**が異なる

    - RUN
        - イメージのビルド時 (docker build)

    - CMD, ENTRYPOINT
        - コンテナの起動時 (docker run や docker start)

<br>

- Dockerfile 内で**記述できる回数**も異なる

    - RUN
        - 何回も記述できる

    - CMD, ENTRYPOINT
        - 1回しか記述できない (実際は複数箇所記述できるが、実行されるのは最後の CMD, ENTRYPOINT)

<br>
<br>

#### CMD と ENTRYPOINT の違い

- 前提となるポイント

    - `docker [container] run` 時に引数としてコンテナに実行させたいコマンドを渡すことができる

        ```docker
        # docker run
        docker run <image> <command> <args>

        # docker container run
        docker container run <image> <command> <args>

        # [例] 作成&起動する Ubunutu コンテナに echo hello world を実行させたい場合
        docker run ubuntu:latest echo hello world
        ```

<br>

- CMD

    - CMD で指定したコマンドは上書きすることができる

        - CMD でコマンドを記述し、かつ `docker run` 時に引数として別のコマンドを渡した場合、**CMD のコマンドは無視され、docker run で指定したコマンドが実行される**

        <img src="./img/Ddockerfile-CMD-ENTRYPOINT-Difference_1.svg" />

<br>

- ENTRYPOINT

    - ENTRYPOINT で指定したコマンドは上書きすることができない

        - ★ **(exec 形式の場合)** ENTRYPOINT でコマンドを記述し、かつ docker run 時に引数として別のコマンドを渡した場合、**docker run で指定したコマンドが ENTRYPOINT のコマンドの引数として解釈され、ENTRYPOINT のコマンドが実行される**

            <img src="./img/Ddockerfile-CMD-ENTRYPOINT-Difference_2.svg" />

        <br>

        - ★ **(shell 形式の場合)** ENTRYPOINT は docker run で渡されるコマンドは無視して、 ENTRYPOINT に記述されたコマンドを実行する

            <img src="./img/Ddockerfile-CMD-ENTRYPOINT-Difference_3.svg" />

<br>
<br>

参考サイト

[【Docker】Dockerfileで記述するRUNとCMDとENTRYPOINTの違いについて](https://www.guri2o1667.work/entry/2021/06/08/【Docker】Dockerfileで記述するRUNとCMDとENTRYPOINTの違いについて)

[DockerfileにおけるCMDとRUNの挙動](https://qiita.com/subretu/items/31a387510f84daaa4c1c)

[DockerfileのENTRYPOINTとCMDの違いは何でしょうか。](https://qiita.com/Thang_TQ/items/acad14dbbc0500fc9d5f)

[DockerfileでのENTRYPOINTとCMDの関係を整理する 〜 2つの記述形式に環境変数展開とシェル引数展開を添えて 〜](https://tech.enechange.co.jp/entry/2022/10/25/100000)

[DockerfileのCMDとENTRYPOINTの違い](https://zenn.dev/asamin/articles/dda05ad3e99fb5)

---

### CMD や ENTRYPOINT で複数コマンドを実行したい

- DockerFile 中で CMD と ENTRYPOINT それぞれ1回しか実行できない

    → では、1回の CMD (ENTRYPOINT) で複数のコマンドを実行したい場合はどうすれば良いのか?

    - 以下の2つの方法で、複数コマンドを1回の CMD (ENTRYPOINT) で実行することができる

        1. [各コマンドを `&`や `|` でつなげて CMD (ENTRYPOINT) に記述する](#や--で複数コマンドをつなげる方法)

        2. [複数コマンドの処理を1つのスクリプトファイルに記述し、それを CMD (ENTRYPOINT) から実行する](#複数コマンドをまとめたスクリプトファイルを実行する方法)

<br>
<br>

#### `&`や `|` で複数コマンドをつなげる方法

- **注意点**

    - exec 形式の場合、コマンドを記号で繋げるシェルの機能はそのままでは利用できないため **/bin/sh などのシェルを起動し、その引数に実行したいコマンドを記述する必要がある**

        
    - exec 形式の場合、`&`, `|` などのコマンドをつなげる記号を別引数として渡してはダメ

    - 全体コマンドを1つの引数として渡すのが正しい

        <img src="./img/Dockerfile-Exec-Multi-Commands_1.svg" />

<br>

- `&`
    - (前のコマンドの終了ステータスに関わらず) **バックグラウンド**で前のコマンドを実行しつつ次のコマンドも実行する

<br>

- `&&`
    - 前のコマンドが正常に終了した場合のみ次のコマンドを実行する

<br>

- `|`
    - 前のコマンドの結果を次のコマンドに渡す
    - 前のコマンドの終了ステータスには関係なく、次のコマンドが実行される

<br>

- `||`
    - 前のコマンドが**異常終了した時のみ**、次のコマンドが実行される

<br>

- `;`
    - 前のコマンドの終了ステータスに関係なく、順番通りに次のコマンドが実行される

    <br>

    ```docker
    # shell形式
    # && でコマンドをつなげる = 前のコマンドが正常に終了した場合のみ次のコマンドを実行する
    CMD <command1> && <command2>

    # exec形式
    # ; でコマンドをつなげる = 前のコマンドが異常終了しても次のコマンドを実行する
    ENTRYPOINT ["/bin/sh". "-c", "<command1> ; <command2> ; <command3>"]
    ```

<br>
<br>

#### 複数コマンドをまとめたスクリプトファイルを実行する方法

- 以下のスクリプトファイル (test.sh) を用意しておく

    ```
    # ディレクトリ / ファイル構成

    .(カレントディレクトリ)
        |- test.sh
        |- Dockerfile
    ```

    ```bash
    # test.sh
    #!/bin/sh

    #command1
    echo "Hello"

    #command2
    echo "from"

    #command3
    echo "shell script"
    ```

    <br>

- CMD (ENTRYPOINT) に、通常のシェルスクリプトを実行するようにコマンドを記述する

- ★実行したいシェルスクリプトをイメージ内にコピー + コピーしたシェルスクリプト権限を実行可能に変更する必要がある

    ```docker
    # Dockerfile

    # ★test.sh をイメージにコピーする
    COPY ./test.sh /test.s

    # ★コピーしたシェルスクリプトの権限を変更
    RUN chmod +x /test.sh

    # シェルスクリプトの実行 (CMD, shell形式の場合)
    CMD /test.sh

    # シェルスクリプトの実行 (ENTRYPOINT, exec形式の場合)
    ENTRYPOINT ["/test.sh"]
    ```

<br>
<br>

参考サイト

[Linuxコマンドを連続して使うには](https://qiita.com/egawa_kun/items/714394609eef6be8e0bf)

[docker起動時にシェルスクリプトを実行する](https://skysan87.hatenablog.com/entry/2021/04/15/215957)

---

### CMD と ENTRYPOINT を両方利用した場合

- **(ENTRYPOINT が exec 形式で記述されている場合のみ)** CMD に記述したコマンドは、ENRTYPOINT への引数として解釈される

    <img src="./img/Dockerfile-CMD-ENTRYPOINT-Both_1.svg" />

<br>

- ENTRYPOINT が shell 形式で記述されている場合は、CMD に記述したコマンドは無視される

    <img src="./img/Dockerfile-CMD-ENTRYPOINT-Both_3.svg" />

<br>
<br>

#### 実験
- ENTRYPOINT に空を記述し、CMD に適当なコマンドを記述すると...

    - ★CMD のコマンドが実行されるが、仕組み的には CMD のコマンドが ENTRYPOINT に渡され、ENTRYPOINT で実行されていることに注意

    <img src="./img/Dockerfile-CMD-ENTRYPOINT-Both_2.svg" />

<br>
<br>

#### ポイント

- CMD の記述形式によって、ENTRYPOINT に渡る引数の形が変わる

    - CMD を **shell 形式**で記述した場合

        - ENTRYPOINT に渡る引数は、`/bin/sh/ -c <CMDのコマンド>` の形になる

            → shell 形式では記述したコマンドの頭に `/bin/sh -c` が付与されるから

        <img src="./img/Dockerfile-CMD-ENTRYPOINT-Both_4.svg" />

    <br>

    - CMD を **exec 形式**で記述した場合

        - ENTRYPOINT に渡る引数は、CMD に記述したコマンドそのままの形になる

        <img src="./img/Dockerfile-CMD-ENTRYPOINT-Both_5.svg" />

<br>
<br>

#### CMD と ENTRYPOINT を両方使うケース

- ENTRYOPINT で実行コマンドを固定し、 CMD でデフォルトの引数を定義する

    - CMD に記述するのはデフォルトの引数なので、別の引数を渡したい場合は docker run / docker container run 時に別途コマンドを指定すればよい

        <img src="./img/Dockerfile-CMD-ENTRYPOINT-Both_6.svg" />

<br>
<br>

#### CMD と ENTRYPOINT と docker run 時にコマンド引数を渡すとどうなるか

- docker run で指定したコマンドは ENTRYPOINT に記述したコマンドの引数として解釈される

    - CMD で記述したコマンドは、docker run で指定したコマンドに上書かれる

    <img src="./img/Dockerfile-CMD-ENTRYPOINT-docker-run_1.svg" />

<br>
<br>

参考サイト

[Dockerfileでよく使う命令一覧【FROM,USER,WORKDIR,COPY,RUN】](https://docker.hatenablog.jp/entry/2023/05/10/190414#CMDの注意点)

[【Docker】Dockerfileで記述するRUNとCMDとENTRYPOINTの違いについて](https://www.guri2o1667.work/entry/2021/06/08/【Docker】Dockerfileで記述するRUNとCMDとENTRYPOINTの違いについて#CMDコマンドとENTRYPOINTの併用時の挙動)

---

### sudo の実行

- ★RUN, CMD, ENTRYPOINT で sudo でコマンドを実行することはできない

    ```docker
    RUN sudo apt udpate

    CMD ["sudo", "echo hello"]
    ```

<br>

- 理由
    - sudo がインストールされていないことが多い

    - (sudo がインストールされていたとしても) sudo 実行時にパスワードの入力を求められるから

<br>
<br>

#### RUN で sudo コマンドを実行しようとすると

- エラーが発生してイメージのビルドが失敗する

    → sudo が見つからないためコマンドの実行に失敗する

    <img src="./img/Dockerfile-sudo_2.svg" />

<br>
<br>

#### CMD, ENTRYPOINT で sudo コマンドを実行しようとすると

- イメージのビルド自体はできる (CMD, ENTRYPOINT のコマンドはコンテナの起動時に実行されるため)

    - しかし、コンテナの起動時に sudo コマンドが失敗する

        → sudo が見つからないため (RUN で sudo コマンドが失敗するのと同じ理由)

<br>
<br>

#### そもそも sudo をつけてコマンドを実行する意味がない

- Dockerfile でのコマンドの操作はデフォルトで root ユーザーであるため、わざわざコマンドに sudo をつける必要がない

    <img src="./img/Dockerfile-sudo_1.svg" />

<br>
<br>

#### 補足: root ユーザーでコンテナを exec　する方法

- `docker exec` コマンドの `-u (--user)` オプションに 0 を指定する

    - こちらの[記事](https://qiita.com/tabimoba/items/c5467432d1a635f9ce5b)がわかりやすい

<br>
<br>

参考サイト

[[Docker] Dockerfileを書く際の知見まとめ](https://qiita.com/EisKern/items/49b76c493fa7d0c9d19d#sudoは使えない)

[dockerfileでRUN sudo〜をつかうとbuildに失敗するときの対処](https://qiita.com/ken992/items/50deb058d390976bd02a)

[Docker：sudoできないDockerコンテナを、rootでシェル操作するには](https://qiita.com/tabimoba/items/c5467432d1a635f9ce5b)

---

### shell 形式と exec 形式どちらを使えば良いのか?

- ★以下のガイドラインは基本的に RUN, CMD, ENTRYPOINT 全てに共通する

- 基本的には exec 形式

    - シェルを噛ませない分高速

    - 指定したコマンドが PID1 で実行されるので、意図した動作を保証しやすいらしい

        - shell 形式で書き, シェルを PID1 で起動すると[こちらの記事](https://takapi86.hatenablog.com/entry/2022/07/03/172254)のようにアプリケーションの停止がまくいかないことがある
    
<br>

- 環境変数の利用やパイプでコマンドをつなぎたい場合は shell 形式を使う

<br>
<br>

参考サイト

[DockerfileのCMDをShell形式で記述したらコンテナが停止しなくなってしまったので直した](https://takapi86.hatenablog.com/entry/2022/07/03/172254)
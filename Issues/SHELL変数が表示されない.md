### 事象

- Dockerfile の CMD 命令について勉強にて、 環境変数 SHELL を echo で表示しようとしても表示されなかった

    - Dockerfile の中身

        ```docker
        # CMD (shell形式) で環境変数を出力してみる
        FROM ubuntu:latest
        CMD echo $SHELL
        ```
    
    <br>

    - Dockerfile から作成したイメージをもとにコンテナの作成&起動した結果

        <img src="./img/Issue-Dockerfile-CMD-SHELL_1.svg" />

---

### 原因

- コンテナ起動時は環境変数 SHELL が設定されていない

- 環境変数 SHELL が設定されるタイミングは、**ログインシェルや対話型シェルが起動したとき**

<br>

#### 検証

- 以下の Dockerfile を作成し、**コンテナ起動時に設定されている環境変数を確認する**

    ```docker
    FROM ubuntu:latest
    CMD printenv
    ```

- 上記 Dockerfile から作成したイメージをもとにコンテナ起動すると、**コンテナ起動時には環境変数 SHELL は設定されていない**ことが見てとれる

    <img src=".//img/Issue-Dockerfile-CMD-SHELL_2.svg" />



---

### 解決方法

- (shell 形式の場合) 基本ない。そもそも環境変数 SHELL を利用して何かしたいケースなんて無い

<br>

- 無理やりな解決策
    - exec 形式の CMD 命令で `/bin/sh -c echo $SHELL` のように、シェルを指定してコマンドを実行する

    ```docker
    FROM ubuntu:latest
    # "/bin/bash" とかでもOK (そのシェルがインストールされているなら)
    CMD ["/bin/sh", "-c", "echo $SHELL"]
    ```
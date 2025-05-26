### PID1 問題とは

- Dockerfile の CMD や ENTRYPOINT に記述されたコマンドがプロセス ID1 (PID1) で起動されることで起きる問題

    - shell形式で記述しても、PID1の問題は回避できない (= /bin/sh を PID1 で動かし、そのほかの PID でメインのプログラムを動かしても PID1 の問題の解決にはならない)

<br>

- 通常の UNIX 系のシステムでは PID0 と PID1 は特別扱いされ、以下のプログラムが動いていることが多い

    - PID0

        - swapper または sched

    <br>
    
    - PID1

        - init というプログラム

        - init は定期的に wait() し、子プロセスの終了をハンドリングする 

        - init はシグナルも受け取りハンドリングする

<br>
<br>

#### PID1 問題の具体的な問題点

- PID1 で動くプログラムが子プロセスの終了のハンドリング (wait()) を実装していないと、[ゾンビプロセス](#ゾンビプロセスとは)を放置する

<br>

- PID1 で動くプログラムがシグナルを自前でハンドリングしないと、シグナルが送信されてもそれを無視する

    - [docker stop](#docker-stop) で SIGTERM を送っても、正しく終了されない

        - docker stop で終了されないプロセスには SIGKILL を送って強制終了する

        - SIGKILL での終了はプロセスの停止に必要な処理は全てスキップされるので、次の起動時にうまく起動しなくなる等のリスクがある

<br>
<br>

#### 解決策

- 以下のようにいくつか方法がある

    1. `docker run` コマンドに `--init` オプションをつけて実行する

        - `--init` をつけることで、コンテナ内で init が PID1 で動くようになる

        - init プログラムはシグナルを受け取る + 子プロセスの終了もハンドリングするため、ゾンビプロセスや、終了の問題も解決する

            <img src="./img/Docker-PID1-Solution_1.svg" />

    <br>

    2. Dockerfile 内の ENTRYPOINT (CMD) で　[Tini](#tini-コマンド) (軽量な init プログラム) を実行するよう記述する (**exec形式で**)

        - 毎回コンテナの実行時に `--init` をつけたくない場合に使える方法 


        ```docker
        FROM <image>:[tag]

        # tini がインストールされていない場合、以下の ADD 命令で tini をインストールすることができる
        ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini

        # 今回の例では tini を / 直下にインストールした
        ENTRYPOINT ["/tini", "--"]

        # tini の引数として実際に実行したいコマンドを記述する
        CMD ["アプリ起動などのコマンド"]
        ```

    <br>

    3. (docker compose で動かす場合) `init: true` を追加する

        ```yml
        services:
          <サービス名>:
            image: <イメージ>:<タグ>
            container_name: <明示的なコンテナ名>
            networks:
              - <ネットワーク名>
            volumes:
              - <ボリューム名>
            init: true #★★起動時に init を PID1 で動かす
        ```

    <br>

    4. (Kubernetes で動かす場合) shareProcessNamespace を有効化する

        - 詳しくは[こちら](https://text.superbrothers.dev/200328-how-to-avoid-pid-1-problem-in-kubernetes/)や[こちら](https://designetwork.daichi703n.com/entry/2023/05/20/concourse-on-kubernetes-zombie-process)を参照

<br>
<br>

参考サイト

[Docker/Kubernetes で PID 1 問題を回避する](https://text.superbrothers.dev/200328-how-to-avoid-pid-1-problem-in-kubernetes/)

[コンテナ内で大量のゾンビプロセスが発生していた話](https://qiita.com/t_katsumura/items/ed105f1c139b24f7fe4f)

[Docker で node.js を動かすときは PID 1 にしてはいけない](https://ngzm.hateblo.jp/entry/2017/08/22/185224)

[DockerfileのCMDとENTRYPOINTを読み解く(1/3) — Shell形式とExec形式とは何か #docker #dockerfile](https://www.creationline.com/tech-blog/cloudnative/docker/39662)

[Docker コンテナ内でゾンビプロセスが発生してしまう問題への対処](https://qiita.com/kentomo1002/items/d2327655dd97f4642be2)

[Dockerと付き合う為の虎の巻 その03](https://www.instance.jp/2016/03/docker-03.html)

[kubernetes podのSIGTERM問題](https://qiita.com/kane8n/items/92b69d464d173198034b)

[Concourse on Docker/Kubernetes環境でのゾンビプロセス対応 (resource temporarily unavailable, [garden-init] <defunct>)](https://designetwork.daichi703n.com/entry/2023/05/20/concourse-on-kubernetes-zombie-process)

---

### ゾンビプロセスとは

- 終了したはずのプロセスが、プロセステーブル上ではその存在が残ったままになってしまう現象のこと

<br>
<br>

#### ゾンビプロセスの原因

<img src="./img/Zombie-Process-Cause_1.jpeg.avif" />

引用: [コンテナ内で大量のゾンビプロセスが発生していた話](https://qiita.com/t_katsumura/items/ed105f1c139b24f7fe4f#ゾンビプロセス発生の仕組み)

- コンテナで動いている PID1 のプログラムが子プロセス終了をハンドリングしていない場合に発生する

<br>
<br>

#### ゾンビプロセスの影響

- ★ゾンビプロセスが CPU や メモリを消費することはない

- しかし、プロセステーブルには居続けるため、PIDは割り振られたまま → PID の枯渇の原因になる

<br>
<br>

参考サイト

[コンテナ内で大量のゾンビプロセスが発生していた話](https://qiita.com/t_katsumura/items/ed105f1c139b24f7fe4f)

[ゾンビプロセス](https://cybersecurity-jp.com/security-words/104668)

---

### シグナルとは

- プロセスに対して送られる信号のこと

- 有名なシグナル
    - SIGTERM はプロセスの終了
    - SIGKILL はプロセスの強制終了

<br>
<br>

参考サイト

[Linuxの「シグナル」って何だろう？](https://atmarkit.itmedia.co.jp/ait/articles/1708/04/news015.html)

[Linux シグナルの基本と仕組み (カーネル v5.5 時点)](https://qiita.com/Kernel_OGSun/items/e96cef5487e25517a576)

---

### docker stop

- docker stop コマンドは、**対象のコンテナの PID1 のプロセスに** SIGTERM を送信する

- ★ docker stop の後 Docker は コンテナ (PID1のプロセス) が終了するまで、デフォルトで10秒間待つ。10秒経過しても終了しない場合は、コンテナのルートプロセス (PID1のプロセス) に対して SIGKILL を送信する。

<br>
<br>

参考サイト

[Dockerコンテナ停止とシグナル（SIGTERM）についての覚え書き](https://eel3.hatenablog.com/entry/2022/11/26/202339)

[Docker で node.js を動かすときは PID 1 にしてはいけない](https://ngzm.hateblo.jp/entry/2017/08/22/185224)

---

### Tini コマンド

```bash
tini <option> -- <args>
```

<br>

- tini はオプションと引数 (args) を受け取る

    - args: tini 起動後に、実行したいコマンド

<br>
<br>

参考サイト

[krallin/tini](https://github.com/krallin/tini)

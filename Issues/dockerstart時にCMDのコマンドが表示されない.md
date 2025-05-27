### 事象

- Dockerfile の CMD に echo コマンドを記述し、`docker container start` でそのコマンドが実行されるか確認したかったのだが、何も表示されなかった

    <img src="./img/Issue-Dockerfile-Dockerstart-CMD_2.svg" />

---

### 原因

- `docker start` コマンドはデフォルトでは標準出力を表示しないため

    - 具体的にいうと、`docker start` コマンドはデフォルトでデタッチモードで実行されるから

    <br>

    <img src="./img/Issue-Dockerfile-Dockerstart-CMD_1.svg" />

---

### 解決策

- `docker start` に `-a` オプションをつけ、[アタッチモード](../Attachコマンド.md)でコンテナを起動する

    <img src="./img/Issue-Dockerfile-Dockerstart-CMD_3.svg" />
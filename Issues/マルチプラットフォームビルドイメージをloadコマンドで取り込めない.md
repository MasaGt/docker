### 事象

- `docker buildx build` コマンドでマルチプラットフォームビルドしたイメージ ([type=oci](../docker_buildコマンドのoutputオプション.md#typeoci)) を `docker load` コマンドで取り込むことができなかった

    <img src="./img/Issue-Load-Non-Docker-Archieve_1.svg" />

---

### 原因

- docker load は Docker (docker-archieve) 形式のみ取り込み可能だから

---

### 問題点

- `docker load` で取り込めるイメージ形式は docker-archieve だが、 build コマンドの [--output type=docker](../docker_buildコマンドのoutputオプション.md#typedocker) でマルチプラットフォームビルドはできない

---

### 解決方法

- そもそもマルチプラットフォームなイメージを local image store は保持できない = (単一の CPU アーキテクチャー向けイメージのみ保持できる)

    - 上記の前提からとれる解決策は以下のように限られてくる

<br>

1. 1つの CPU アーキテクチャ (プラットフォーム) ごとにイメージをビルドする ([--load](../docker_buildコマンドのoutputオプション.md#--load-オプション-buildx-build-のその他のオプション) か [--output type=image](../docker_buildコマンドのoutputオプション.md#typeimage) でビルド)

<br>

2. [skepto](https://github.com/containers/skopeo) などのツールを利用する

    - skepto: マルチプラットフォームな OCI 形式のイメージから特定のプラットフォーム向けの Docker 形式のイメージをビルドすることができる CLI ツール

<br>

3. そもそも外部ファイルとして出力するのをやめ、リモートレジストリに push する ([--push オプション](../docker_buildコマンドのoutputオプション.md#--push-オプション-buildx-build-のその他のオプション)をつける)
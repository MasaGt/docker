### 事象

- `docker buildx build` コマンドでマルチプラットフォームビルドしたイメージ ([type=tar]()) を `docker load` コマンドで取り込むことができなかった

    <img src="../img/Issue-Loading-Mutli-Platform-Built-Image_1.svg" />

---

### 原因

- docker load は Docker (docker-archieve) 形式のみ取り込み可能だから

---

### 問題点

- `docker load`で取り込めるイメージ形式は docker-archieve だが、マルチプラットフォームイメージと docker-archieve は食い合わせが悪い

    - `docker buildx build` コマンドで type=docker を指定すると出力するイメージ(のアーカイブ)は Docker (docker-archieve) 形式になる

        - 問題点: マルチプラットフォームビルドをサポートする docker-container ドライバーは docekr-archieve 形式のイメージビルドをサポートしない = docker-archieve 形式のイメージをマルチプラットフォームビルドできない

    <br>

    <img src="../img/Issue-Loading-Mutli-Platform-Built-Image_2.svg" />

---

### 解決方法

以下のようにいくつか方法がある

1. 1つの CPU アーキテクチャ (プラットフォーム)ごとにイメージをビルドする

    <img src="../img/Issue-Loading-Mutli-Platform-Built-Image_3.svg" />

<br>

2. [skepto](https://github.com/containers/skopeo) などのツールを利用する

    - skepto: マルチプラットフォームな OCI 形式のイメージから特定のプラットフォーム向けの Docker 形式のイメージをビルドすることができる CLI ツール

<br>

3. そもそも外部ファイルとして出力するのをやめ、リモートレジストリに push する (--push オプションをつける)

---

### Docker (docker-archieve) 形式イメージの構造

- ★以下は古い docker-archieve 形式

    <img src="../img/Docker-Image-Format_1.svg" />

<br>

- Docker 25 以降にて docker-archieve 形式は OCI と互換性があるように以下の形式になったらしい
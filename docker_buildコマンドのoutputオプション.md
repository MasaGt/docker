### --output オプション

```docker
docker (buildx) buid --output [params]
```

- 省略形は -o

```docker
docker (buildx) buid -o [params]
```

---

### type パラメーター

- ビルドしたイメージの出力形式を指定する項目

- ★[docker ビルドドライバー](./buildxコマンド.md#build-driver) で利用できる type オプションは [docker](#typedocker), [local](#typelocal), [tar](#typetar)

<br>
<br>

### type=local

- Dockerfile をビルドした結果、COPY や RUN の成果物として最終ステージに残ったファイルやディレクトリを出力する

    - 出力結果はディレクトリ

        <img src="./img/Image-Type-Local_1.svg" />

```
--output type=local,dest=<出力先>
```

- ##### type=local で指定できる属性

    - dest: 出力先 (省略不可)

<br>

- ★出力結果が docker-archieve 形式ではないなので、`docker load` コマンドで取り込めない

    <img src="./img/Image-Type-Local_2.svg" />

<br>
<br>

### type=tar

- [type=local](#typelocal) の出力結果を tar でアーカイブするようなオプション

    <img src="./img/Image-Type-Tar_1.svg" />

```docker
--output type=tar,dest=<出力先>
```

- ##### type=tar で指定できる属性

    - dest: 出力先 (省略不可)

<br>

- type=local と同様に、出力結果を `docker load` でロードできない

<br>
<br>

### type=oci

- ビルドしたイメージを OCI 形式のイメージアーカイブで出力するオプション

    <img src="./img/Image-Type-OCI_1.svg" />

```docker
--output type=oci[,attributes]
```

- ##### type=oci で指定できる属性

    - dest: 出力先 (省略不可、 - を指定すると標準出力にストリームする)

    - その他に指定できる属性については[こちら](https://docs.docker.com/build/exporters/oci-docker/)を参照

<br>

- docker ドライバでは利用できない 

<br>

- `docker load` コマンドでロードできない

<br>
<br>

### type=docker

- ビルドしたイメージを docker-archieve 形式のイメージアーカイブで出力するオプション

    <img src="./img/Image-Type-Docker_1.svg" />

```docker
--output type=docker[,attributes]
```

- ##### type=docker で指定できる属性

    - 基本的には [type=oci](#typeoci) で利用できる属性と同じ

    - dest: 出力先 (type=oci とは違い省略可。省略した場合は、local image store に保存され docker images / docker image ls で確認することができる)

<br>

- ★type＝docker を指定する場合、マルチプラットフォームビルドはできない

    <img src="./img/Image-Type-Docker_2.svg" />

<br>

- docker ドライバで type=docker でビルドしたイメージをを local image store に保存することはできるが、**destを指定した外部ファイルへの出力はできない**

    <img src="./img/Image-Type-Docker_3.svg" />

<br>
<br>

### type=image

- ★ビルドしたイメージを local image store に出力するオプション

```docker
--output type=image[,attributes]
```

- ##### type=image で指定できる属性

    - (省略可) name=<イメージ名>:<タグ> → -t で指定するのと同じ内容 (name 属性と -t の両方でイメージ名:タグ が付与されている場合、 -t の方が適用される)

    - その他に指定できる属性については[こちら](https://docs.docker.com/build/exporters/image-registry/)を参照

        - ★dest 属性は指定できない = local image store に出力 or リポジトリにプッシュしか出力方法は無い

<br>

- ★docker ビルドドライバーにて、--outputコマンドを指定しない時は type=image がデフォルト

<br>

- ★★docker ビルドドライバー以外では、 type=image の後に [--load オプション](#--load-オプション-buildx-build-のその他のオプション)か [--push オプション](#--push-オプション-buildx-build-のその他のオプション)を付けないとビルド結果は出力されない

    <img src="./img/Image-Type-Image-Docker-Container_1.svg" />

    <br>

    - しかし、--load の実態は type=docker なので、 type=image が type=docker に上書きされるだけ (--push の場合も同様に type=registry に上書きされるだけ)

        - --load (--push) と --output の順番は問わない

            <img src="./img/Image-Type-Image-Docker-Container_2.svg" />

<br>
<br>

### type=registry

- 指定したレジストリにビルドしたイメージをプッシュするオプション

```docker
--output type=registry[,attributes]
```

- ##### type=registry で指定できる属性

    - 基本的には [type=image](#typeimage) で指定できる属性と同じ

<br>

- ★type=registry でのマルチプラットフォームビルド自体は可能

    - push 対象のレジストリもマルチプラットフォームイメージのサポートをしている必要がある

    - また、イメージを push する前には対象のレジストリに `docker login` コマンドでログインしておく必要がある

        <img src="./img/Image-Type-Registry_1.svg" />

<br>
<br>

### type=cacheonly

- ビルド結果をどこにも出力しないオプション

<br>

```docker
--output type=cacheonly
```

<br>

- docker ドライバー**以外**のビルドドライバーを利用していて --output を指定しない場合は、 type=cacheonly がデフォルトになる

<br>

- type=cacheonly だけではビルド結果は出力されない

    - --load オプションを付与することで、local image store に出力

    - --push オプションを付与することで、レジストリにプッシュする
    
<br>
<br>

### 複数の type を指定する (=複数の exporter の利用)

- 1回のビルドで複数の image exporter (type=oci,docker,image など) を指定することもできる

- 利用する exporter ごとに --output (-o) オプションで指定する

    ```docker
    docker buildx build \
    --output type=<exporter>[,parameters] \
    --output type=<exporter>[,parameters] \
    ...
    ```

    <br>

    <img src="./img/Build-Multi-Exporter_1.svg" />

<br>
<br>

参考サイト

[docker buildx build](https://docs.docker.com/reference/cli/docker/buildx/build/#output)

[Exporters overview](https://docs.docker.com/build/exporters/)

[docker build](https://docs.docker.jp/engine/reference/commandline/build.html#id9)

[OCI and Docker exporters](https://docs.docker.com/build/exporters/oci-docker/)

[Multiple exporters](https://docs.docker.com/build/exporters/#multiple-exporters)

---

### --load オプション (buildx build のその他のオプション)

- --output type=docker のショートハンドラ

---

### --push オプション (buildx build のその他のオプション)

- --output=type=registry のショートハンドラ
### Dockerfileとは
- Docker Imageを作るための元ネタ
- Docker Imageの定義ファイルで、Docker FileをビルドすることでDocker Imageが作成される

---

#### Dockerfileの記述例

```
FROM イメージ名:バージョン
(COPY コピー元パス コピー先パス)
(ADD コピー元パス コピー先パス)
(その他のコマンドもある)
```

*FROMでイメージのバージョンを指定しない場合はlatestが暗黙的に指定されたことになる  
*コピー元パス(Dockerfile内のファイルの相対パス)  
*コピー先パス(コンテナ内の絶対パス)  


**ADDとCOPYコマンドの違い**  
COPY はソース側のマシンにあるものをImageへコピーするだけというシンプルな機能であるのに対し、 ADD は対象が圧縮ファイルであればそれを展開してImageへ移し、リモート上のリソースも引っ張ってくる機能がついています

---

### Dockerfile 実行時のコンテナ内のパス設定

```
FROM <イメージ名>:<バージョン>

WORKDIR <コンテナ内のパス>

#　RUNやADDなどDocker instructionが実行されるディレクトリがWORKDIRで設定された<コンテナ内のパス>になる
```

\*このファイルを元にイメージを作成->コンテナ作成した場合、**そのコンテナのデフォルトのパスがWORKDIRで設定したパスになる**

例:
```
# DockerFile側

FROM ubuntu

WORKDIR /home

# 以降省略
```

<br>

```bash
# ターミナルにて
docker build -t <作成するイメージ名>:<バージョン> <.(DockerFileのあるディレクトリ)>

# 作成したイメージからコンテナ作成
docker run --name <コンテナ名> -dit <イメージ名>

# コンテナ内に入る
docker exec -it <コンテナ名>

# コンテナ内での現在のパスを確認
pwd

# /home (docker pull ubuntuから作成するコンテナでは/)
```


**コンテナ内のデフォルトのパスを / にしたい場合は、Dockerfile内で、作業instruction終了後にWORKDIR / を記述する**

```
# DockerFile側

FROM ubuntu

WORKDIR /home

# 以降省略

WORKDIR /
```

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

# 目次

1. [事象の要約](#sec1)

2. [原因](#sec2)

3. [解決策](#sec3)

---
<a id="sec1"></a>

### 要約

image: https://hub.docker.com/_/node/

[準備]
- 上記リンクのnodeコンテナのイメージをpull

[問題]
- nodeコンテナをバインドマウントしてrunするとエラー

```bash
# ReactAppというreactプロジェクト用ディレクトリをユーザーディレクトリ直下に作った
docker run --name aaa -dit -v /Users/<ユーザー名>/ReactApp:/usr node:lts-slim
```

[エラーメッセージ]

```
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "docker-entrypoint.sh": executable file not found in $PATH: unknown.
```

[他の事象]
- -v ローカルのディレクトリ:/tmp でrunはできた

---
<a id="sec2"></a>

### 原因

-v ホスト側のディレクトリ:コンテナ側のディレクトリ を指定すると、  
<font color="red">コンテナ側のディレクトリはマウント元のホスト側のディレクトリで上書きされる</font>  
つまり、コンテナ側のディレクトリにすでにファイルやフォルダが存在する場合、それらがマウントの際に消えてしまう


```
docker run -v <ホスト側のディレクトリ>:/usr node:lts-slim
```
上記コマンドはホスト側のディレクトリの内容で/usrディレクトリを上書こうとしていた。  
usr 配下には大事なフォルダが存在しており、消されるとまずい  
また、<font color=red>nodeコンテナのルートディレクトリには/binがあり、それがシンボリックリンクで/usr/binを指していた。いくつか同様にusr配下のフォルダへのリンクをもったフォルダがあった。ボリュームマウントの際にusr 配下にあるこれらのリンク元フォルダを消す際にエラーがでた?</font>

---
<a id="sec3"></a>

### 解決策

- コンテナ内の重要なファイルやディレクトリが存在するディレクトリをマウント先に指定しない

- コンテナ内に新しくディレクトリを作り、そこをマウント先に指定すれば何も削除されなくて済む

例
```bash
docker run --name aaa -dit -v /Users/<ユーザー名>/ReactApp:/usr/app node:lts-slim

# nodeコンテナの usr 配下に app というファイル/ディレクトリはないので、上記コマンド実行の際にdocker側で app ディレクトリを作成してマウントしてくれる
```
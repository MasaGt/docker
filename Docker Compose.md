### Docker Composeとは
Dockerへの命令を定義ファイルに記述して実行できるツール  
→Dockerfileに似ている?

---

#### Dockerfileとの違い
Dockerfile: イメージを作成するための定義を記述したファイル

Docker Compose:複数のコンテナの定義(使うボリュームやネットワークなど)を記述でき、それを一気に実行できる

まとめ：  
Docker fileをイメージを作成  
Docker Composeはコンテナを作成


---

#### Docker Composeで何がうれしいのか?
複数のアプリ(コンテナ)で構成されるシステムの環境構築が簡単にできる。

---

#### Docker Composeの定義ファイル
1. Docker Composeの定義ファイルはYAML形式

2. 定義ファイルに各コンテナの定義(使うボリュームやネットワークなど)を記述する

3. docker up コマンドで定義ファイルを実行する

4. docker down コマンドでコンテナ、ネットワークなどを停止・削除する(ボリュームは残る)

もしくは docker stop コマンドでコンテナ、ネットワークを停止する(コンテナの削除は行われない)

---

#### Docker Compose と Kubernatesの違い

Kubernatesは複数のコンテナの管理をするソフト

Docker Composeは複数のコンテナ(ネットワーク・ボリューム)を作成・削除できるツール

Docker Composeにはコンテナの管理機能は無い

---

#### 注意点

Docker Composeでコンテナ作成後、そのコンテナに変更を加えても、定義ファイルには反映されない。

起こりうる問題:  
Docker Composeでコンテナ作成後にDockerコマンドでコンテナ名を変更した。  
→ Docker Composeでコンテナの停止/削除を仕様としても定義ファイルに書かれているコンテナ名が見つからなく、失敗する。

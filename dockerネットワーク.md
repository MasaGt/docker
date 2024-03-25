### dockerネットワークコマンドとは
コンテナ間でコミュニケーションできるように、仮想的なネットワークを作成/管理/削除できるコマンド

---

#### ネットワークを作成する

        docker network create [ネットワーク名]

---

#### コンテナを作成したネットワークに接続する

- docker networkコマンドを使用する方法

        docker network connect [ネットワーク名] [コンテナ名]


- コンテナ側から接続する方法(--net　オプションを使う)

        docker container run -net [ネットワーク名] [コンテナ名]

---

#### 仕組みと使い方...　p131のsqlとワードプレスコンテナの接続にハマった

- 原因  
wordpressコンテナ作成の際に、環境変数として-e WORDPRESS_DB_HOSTに適当にhostと設定してしまっていた。

- 解決方法  
WORDPRESS_DB_HOSTには、sqlコンテナ名を指定する

---

#### ネットワークの確認方法
        docker network inspect [ネットワーク名]

Containersにそのネットワークに接続しているコンテナ名が表示される。
**(起動しているコンテナのみ)**

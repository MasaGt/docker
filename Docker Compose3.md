#### ネットワークへの接続

定義ファイル内で、networks にネットワーク名を指定し無くても、コンテナ同士をつなぐことができる

servicesのコンテナ定義に links で接続するコンテナ名を指定する

        services:
          [コンテナ名1]
          ...

          [コンテナ名2]
          　　links:
                - [コンテナ名1]

docker-compose up すると、[定義ファイルのディレクト名]_defaultというネットワークが作成されている。

**しかし、明示的にネットワーク名を指定して、各コンテナ定義にどこのネットワークに接続するのかを指定した方がいい**

---

#### volumesの指定

定義ファイル内でvolumesを指定しなくても、必要なボリュームは勝手に作成される

        services:
          sqlコンテナ
          ...(volumesの指定なし)

docker volumes ls で確認すると、ボリューム名がハッシュ値のsql用のボリュームが作成されている。

**networks同様に、明示的にvolumesにボリュームを定義し、各コンテナ定義に、どのボリュームを使用するのかを指定した方がいい**

---

### volume mount の書き方の注意点

- ボリュームマウントで volumes を設定する場合、 ボリューム名:コンテナのディレクトリ の間にスペースを入れてはダメ

<br>

例: var/lib/mysql に myvolume と名前をつけてボリュームマウントする場合

```yml
# ダメな例
services:
  container_name:
    volumes:
      - myvolume: /var/lib/mysql

# いい例
services:
  container_name:
    volumes:
     - myvolume:/var/lib/mysql
```
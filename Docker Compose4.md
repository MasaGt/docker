### Docker Composeで作成されるコンテナ名

- Docker Compose で作成されるコンテナ名は**[ディレクトリ名]-[コンテナ名]-[連番]**で作成される

- ネットワーク名やボリューム名にも同じような感じで[ディレクトリ名]_[ネットワーク/ボリューム名]で作成される

---

### 明示的にコンテナ/ネットワーク/ボリューム名をつける　

```yml
services:
  <コンテナ名>:
    image: <イメージ名>:<バージョン>
    container_name: <明示的なコンテナ名>
    networks:
      - <ネットワーク名>
    volumes:
      - <ボリューム名>
networks:
  <ネットワーク名>:
    name: <明示的なネットワーク名>
volumes:
  <ボリューム名>:
    name: <明示的なボリューム名>
```

*docker psで<明示的なコンテナ名>でコンテナが作成されていることを確認する

*docker network lsで<明示的なネットワーク名>でネットワークが作成されていることを確認する

*docker volume lsで<明示的なボリューム名>でボリュームが作成されていることを確認する

---

### 実際の例

```yml
services:
  ubuntu_container:
    image: ubuntu
    container_name: ubuntu
    networks:
      - sample_net
    volumes:
      - sample_vol
networks:
  sample_net:
    name: net1
volumes:
  sample_vol:
    name: vol1
```
### --env-file　オプション
- コンテナの環境変数を設定する方法はいくつかある
    1. -eオプションを使い 環境変数名=値 で環境変数をコマンドから設定する
    ```bash
    docker run -e 環境変数名=値 <イメージ名>
    ```

    2. --env-fileオプションを使い 外部ファイルから環境設定を読み込む  
    ```bash
    docker run --env-file <envファイルのパス> <イメージID>
    ```

    ```
    <!-- .env -->
    <!-- MYSQLの定義の例 -->
    MYSQL_ROOT_PASSWORD=root
    MYSQL_DATABASE=database
    MYSQL_USER=user
    MYSQL_PASSWORD=user
    ```
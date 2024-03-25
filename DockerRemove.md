### rmコマンド

- コンテナの削除  
    稼働中のコンテナは削除できない  
    docker (container) stop <コンテナ名 or ID>で停止してから  
    docker (container) rm <コンテナ名 or ID>

<br>

- イメージの削除  
    ターゲットのイメージを元にしたコンテナが存在すると、イメージを削除できない  
    docker ps -a でコンテナのIMAGE項目を確認する  
    削除対象のイメージをから作られたコンテナが存在しない場合  
    docker (image) rm <イメージ名 or ID>で削除する


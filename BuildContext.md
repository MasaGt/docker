### ビルドコンテキストとは

- Docker イメージを作成（ビルド）するときに Docker に渡す「作業用のフォルダ」のようなもの

<br>

- ★★★Docker は `docker build` コマンドを使ってイメージを作るときに、
その時のビルドコンテキスト内のファイルだけを使ってビルドする

<br>

- ★ビルドコンテキストは1つしか指定できない

<br>
<br>

参考サイト

[docker buildコマンドとは？ビルドコンテキストとは](https://penpen-dev.com/blog/docker-build-command/)
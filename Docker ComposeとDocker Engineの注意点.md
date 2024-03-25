#### コンテナ名の違い

docker-composeでコンテナを作成すると、コンテナ名が  
**[定義ファイルのあるディレクトリ名]-[定義したコンテナ名]-[
連番]**  
となる  

これは、Docker Engineでコンテナの操作ができるようにするための仕組みらしい  

ネットワーク名やボリューム名にも同じような感じで[ディレクトリ名]_[ネットワーク/ボリューム名]となる

- docker-compose コマンドでコンテナを操作する場合  
  [定義したコンテナ名]で操作できる  
  (むしろ、↑の自動的に生成されたコンテナ名で指定すると操作出来ない)

- docker コマンドでコンテナを操作する場合
  自動生成されたコンテナ名で操作できる

---

例:  
pracというフォルダにdocker-compose.ymlを作成し、定義ファイル内でsampleという名前のコンテナを定義  

docker-compose up でコンテナを作成  
→プロンプトに prac-sample-1 というコンテナが作成されたと表示される。

---

**docker-compose で操作する場合**

        docker-compose log sample

でログを見れる

        docker-compose log prac-sample-1

では「そんなコンテナ無い」と言われる


**docker コマンドで操作する場合**

        docker log prac-sample-1

でログを見れる

        docker log sample

では、「そんなコンテナ無い」と言われる

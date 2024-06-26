### tarkコマンド
- 複数のファイルをまとめて1つのアーカイブファイルを作成するためのコマンド

- **tar <オプション> <アーカイブ名>.tar <対象ファイル or フォルダ>**

<br>

| オプション | 説明 | 補足 | 
|   :---   |  :-  |  :-  | 
| c | 新規にアーカイブを作成する | createの略 | 
| v | 圧縮・展開時に詳細を表示する | verboseの略 | 
| f | アーカイブファイル名を指定する  | これを指定しないと標準入力 または標準出力が使われる | 
| z | gzipの圧縮または展開 | この時のアーカイブフォルダの拡張子は.tar.gz または .tgz | 
| t | アーカイブファイルに含まれるファイルの一覧を表示する | listの略 | 
| x | アーカイブを展開 | extractの略  | 

<br>

例
```bash
tar cfz archive.tar.gz ./target_dir
```

### 圧縮したアーカイブを解凍・展開する
- xオプションで解凍・展開する
- gzipの時はzxで展開する　　

例
```bash
tar fzx archive.tar.gz
```

### アーカイブの中身を確認する
- tオプションを使う

例
```bash
tar ft archive.tar.gz
```
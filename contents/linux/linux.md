- Linux とは何か？
  単に Linux というと Linux カーネル、Linux ディストリビューションを指す
  基本的には Linux OS の意味で使われる
  OS の 1 つである
  OS の中核である Linux カーネルはオープンソースで開発されており、安定性も高いため、サーバー用 OS として使用されている

- OS とは何か？
  コンピューターを動作させる中核となるソフトウェアファイル群のこと
  OS が提供するものはデスクトップ環境、ファイルシステム、ネットワーク通信、プロセス間通信、プロセス・メモリ・デバイスの管理、シェル、コマンド
  OS の中核にはカーネルというソフトウェアが存在する

- Linux カーネルとは何か？
  Linux において OS の中核となるソフトウェアのことを指す
  プロセス管理、メモリ管理、デバイス管理などを行う
  OS の一部の機能を担っている

- 相対パスと絶対パス
  絶対パスは変わらないのと、長くなること、ルートディレクトリから常に辿るため、環境に依存しやすい。
  相対パスはカレントディレクトリをわかっていないといけないが短くて済む

- ターミナル , プロンプト、コマンドライン、
  ターミナルを通じ、コマンドラインにコマンドを入力してシェルというソフトウェアへ命令を伝えている
  シェルはユーザーのコマンドを解釈して実行するソフトウェアのこと。

- どういう流れか
  ls を入力するとシェルが ls という名前のコマンドに対応するファイルを linux から探す
  そして linux カーネルに実行させる(ls ファイルを渡す)
  シェルはその結果を受け取り、ターミナルへ出力している

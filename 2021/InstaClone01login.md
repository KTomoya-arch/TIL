## 復習ポイント
コマンドをターミナルで打つだけで色々と環境を整えてくれてしまうRailsなので、今回は一つ一つコマンドが何をやっているのかという点も含めて復習ポイントにした。正直この辺りが理解できていないのが個人的には気持ち悪かったため。

- bundle init
<br>
作業中のディレクトリに、Gemfileを召喚する。
- bundle install
 <br>
Gemfileに記載されたGemfileをインストールする。
- Gemfile
<br>
Railsアプリで利用するgemの一覧を管理するファイル、ライブラリ。redis、slimなどなどRailsに標準搭載されていないが使用したいgemがある場合はとにかくここにgem 'gem名'と書いてbundle installすればいいような感じか。VisualStudioでいうところのnugetで取ってくる参照みたいなもの。
- bundler
<br>
導入しようとしているgem同士の間の依存関係やバージョンを管理するgem。bundlerを使用することで依存関係にあるようなgemがあったとしてもそれを意識せずに勝手に一括でインストールしてくれる。（最強）
- --path vendor/bundle
<br>
https://qiita.com/jnchito/items/99b1dbea1767a5095d85
<br>
このオプションを付けないと、システム全体にgemがインストールされる。
上記ページに記載があるように記載する理由は色々あるがつけた方が問題が発生する可能性は低くなるとは言えそう。
- git flowとは
<br>
Gitのブランチを使用した開発手法の１つ。複数名による開発のコンフリクトやマージミスなどを回避するために使用する。feature,develop,release,master,hot-fixブランチのように分け、各ブランチごとに役割・目的が明確になっている。インスタクローンはリリースしないのでfeature,develop,masterがあり、各機能（01〜18）作成してはdevelopブランチへプルリク・マージする感じか。
- turbolinkとは
<br>
仕組みはいまいち理解できていないがこの仕組みによってjQueryのreadyイベントやWindowのloadイベントの発生を邪魔してしまうよう。HTMLをロードしたときに起こるはずのイベント等々が阻害されてしまうくらいのイメージで。
- slim
<br>
ビューテンプレート。閉じタグがないのでスタイリッシュなhtmlを書くことができる。インデントによって親子関係を調整している。ただ慣れるのに少し時間がかかりそう。
- sorcery
<br>
ログイン機能を実装するためのgem。インストールすると、Userモデル・マイグレーションファイルなどが生成される。
loginメソッド、logoutメソッド、logged_in?、redirect_back_or_toメソッド等をsorceryのcontroller.rbが有している。
crypted_password(暗号化されたパスワード)があるのでsorceryにパスワードを暗号化してテーブルに表示しない仕組みがあるのだろう。
この辺りも見えないのが・・・。
- redis
データの保存にメモリを使用するため高速である。有効期限のあるセッションを扱うのに適している。いまいちしっくりきていなかったので一応保存されているデータを確認。インスタクローンにログインしたのち、ターミナルでredisサーバへアクセスして対象となる0のDBのkeysを確認。![代替テキスト](https://i.gyazo.com/c66c3edf0a6c6b70a70e6deb07caf6a4.png)
- annotate
<br>
![](https://i.gyazo.com/b5e15f2a5d373d0d67664798fa9f26c3.png) <br>
rails db:migrateを実行したタイミングでスキーマの情報をモデルに書き込んでくれる。個人的にはスキーマを見に行くのも面倒だったので非常にありがたい。アノテーションにはキーワードもあり、(TO DO,FIX ME, REVIE...)と確認観点が明確になる。またルーティングを書き込むこともできるよう。
- rails-i18n
<br>
アプリケーションのエラーメッセージなどをある言語から別の言語へ翻訳するための機能。
- database.yml
<br>
Railsにおけるデータベースの設定ファイル。デフォルトの設定ではSQLiteになっている。そもそもYAMLとは何かと思ったがYAMLというルールに従ったテキストファイルくらいの認識で良さそうで、そのファイルには.ymlがつく。自分の身近な例でいけば頻繁に使用する.xmlファイルに近いと思っておこう。SQLite以外のDBを使用する場合はrails newする際に　-d オプションの後にDBを指定する必要あり。
- migrationファイル/schema.rb
データベースを生成するためのRubyで書かれたテーブル設計図のこと。DBに操作を行いたい場合は全てマイグレーションファイルを使用する。rails db:migrateコマンドによって作成したマイグレーションファイルが実行される。マイグレーションにはステータスがあり、実行済はup、そうでない場合はdownの状態を持つ。（rails db:migrateによってdownからupになり、upのものは変化しない。rails db:rollbackで最新のマイグレーションをupからdownにすることもできるし、複数のマイグレーションを指定することも可能。）schema.rbはテーブルの最新のレコードversionが記録されている。(今回はそれをmodelにannotateもしている。)
- config/application.rbとは
<br>
アプリケーション全体に関わる設定を記述するファイル。デフォルトのロケールやタイムゾーンの設定をここで書く。

```
    config.load_defaults 5.2
    config.time_zone = 'Tokyo'
    config.active_record.default_timezone = :local
    config.i18n.default_locale = :ja
```
i18nの設定もここでen.ymlからja.ymlの指定に変更している。
- yarnとは
<br>
Node.jsのパッケージ管理マネジャー。RailsにおけるBundlerと似たようなイメージ？とにかくこいつを使用すればJavaScript関連のものを色々管理できるといった感じだろうか。





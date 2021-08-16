- ユーザーとポストのシードファイルを作る

  ### シードファイル(seed.rb)とは？

  初期データを作成するためのファイル。

  作成したアプリケーションの処理が正常に動くかどうかを確認する際に、DBにデータを用意するが、そのデータ作成の手間をseed.rbを作成・使用することで減らすことができる。

- その際fakerを使ってダミーテキストを生成する

  ### fakerとは？

  実際に存在しそうなデータをランダムで生成するgem。

  #### 導入手順

  `$ gem 'faker'`

  `db/seeds.rb`を書き換える。

- 画像のアップロードにはcarrierwaveを使用する

  ### carrierwaveとは？

  ファイルのアップロード機能を簡単に追加する事ができるgem。

  #### 導入手順

  [参考サイト](https://pikawaka.com/rails/carrierwave)

  #### Rails実践ガイドで使用したActiveStorageとの違いは？

  https://qiita.com/w5966qzh/items/510d4c2a3829524b2e64

- image_magickを使用して、画像は横幅or縦幅が最大1000pxとなるようにリサイズする

- 画像は複数枚アップロードできるようにする

- Swiper使って画像をスワイプできるようにする

- 諸々のアイコンにはfontawesomeを使用する
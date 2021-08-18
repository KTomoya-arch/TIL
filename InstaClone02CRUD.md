1. ユーザーとポストのシードファイルを作る
	- シードファイル作成にあたって投稿に関するテーブル、モデルが必要であると気づく
	- Postのモデルを作成する
	- PostモデルとUserモデルの関連付けを行う(Userと投稿には結び付きがある。）
	  has_many , belongs_to
	  
	- シードファイルの作成をし、rails db:seed (seed.rb)
	
	```
	User.create(username: 'seeduser', email: 'sample@sample.com', password: 'pass', password_confirmation: 'pass')
	```
	![](https://i.gyazo.com/b15409300a63566422dde257bec020c8.png)
  
2. その際fakerを使ってダミーテキストを生成する

```
50.times do
     User.create(
        name: Faker::DragonBall.character,
        email: Faker::Internet.email,
        postcode: Faker::Address.postcode,  #integer
        address: Faker::Address.city,
        admin: Faker::Boolean.boolean       #boolean
      )
end
```

初めは上の書き方で書いたが以下のエラー。
![](https://i.gyazo.com/b1aff3509752ff3a17859bdb97884908.png)
なんとなく予想はしていたが多分インストールしたgemに`Faker::DragonBall.character`のような定義はないのだろうと思いつつvendorのfakerのlibの中を探っていくと・・・やはりか。

```
module Faker
  class JapaneseMedia
    class DragonBall < Base
      class << self
        ##
        # Produces the name of a character from Dragon Ball.
        #
        # @return [String]
        #
        # @example
        #   Faker::Games::DragonBall.character #=> "Goku"
        #
        # @faker.version 1.8.0
        def character
          fetch('dragon_ball.characters')
        end
```
`Faker::DragonBall.character`を`Faker::Games::DragonBall.character`にして解決。
コピペを完全に信じてはいないがうまくいっていない場合はちゃんと自分の設定ファイルや定義ファイルを確認しようと思った。
もちろんドラゴンボール愛

3. 画像のアップロードにはcarrierwaveを使用する

4. image_magickを使用して、画像は横幅or縦幅が最大1000pxとなるようにリサイズする

5. 画像は複数枚アップロードできるようにする

6. Swiper使って画像をスワイプできるようにする

7. 諸々のアイコンにはfontawesomeを使用する

### シードファイル(seed.rb)とは？
  初期データを作成するためのファイル。
  作成したアプリケーションの処理が正常に動くかどうかを確認する際に、DBにデータを用意するが、そのデータ作成の手間をseed.rbを作成・使用することで減らすことができる。
### fakerとは？
実際に存在しそうなデータをランダムで生成するgem。
#### 導入手順

  `$ gem 'faker'`

  `db/seeds.rb`を書き換える。
### carrierwaveとは？

  ファイルのアップロード機能を簡単に追加する事ができるgem。

#### 導入手順
`gem carrierwave`

`$ bundle exec rails g uploader image`



  [参考サイト](https://pikawaka.com/rails/carrierwave)

#### Rails実践ガイドで使用したActiveStorageとの違いは？

  https://qiita.com/w5966qzh/items/510d4c2a3829524b2e64
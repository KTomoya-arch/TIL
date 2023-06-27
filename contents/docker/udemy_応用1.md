## AWS を使用してコンテナを起動する

`$ ssh -i ****.pem ubuntu@<EC2の外部DNS>`

ssh では秘密鍵(クライアント)を用いて公開鍵暗号方式を行う。サーバー側に公開鍵を置いておく。
上記の ssh コマンドの-i は、identity の意味で引数に鍵を取る。

docker を ubuntu にインストールしたのは良いが、sudo 権限でインストールしたので
ubuntu ユーザーには docker を実行する権限がない。そのため、docker というユーザーグループを作成し
権限を付与し、そのグループに ubuntu ユーザーを入れる。
さらにログインをし直す必要もあり。

### docker save imageID > \*\*\*.tar

docker イメージを tar ファイルに変換する

### sftp コマンド

secure file transfer protocol
put コマンド、get コマンドが存在しており、リモートからローカルまたはローカルからリモートへファイルを送信することができる。
ftp をセキュアにしたもの？

### /var/lib/docker

一般的に docker を linux で起動すると、
`/var/lib/docker`が docker オブジェクトで埋まっていく。
そこの容量が足りないと run できない。

### docker-compose

docker コンテナをどのように記述するか、つまり docker run のコマンドで指定するオプション周りの記述をメインに行う。
具体的に使用するケースは

- 複数のコンテナを同時に立ち上げるとき
- docker run のコマンドが複雑にオプションを指定しなければならない時

docker-compose に指定する、volume のパスについては誰がローカルにインストールしても良いように、フルパスでは書かず、相対パスで記述する

### docker-compose コマンド

$ docker build <build contexts> $ docker-compose build
$ docker run <image> $ docker-compose up
$ docker ps $ docker-compose ps

#### 便利なコマンド

$ docker-compose up --build: build して run もする。
$ docker-compose down: stop して rm する。

### ファイルマウント

postgresql などを docker コンテナで起動した場合、その DB のデータはコンテナを消去すると通常なくなってしまう。
そのため、ファイルやディレクトリをホスト側とコンテナ側で共有する必要がある。

- ホストマシン側で持つ場合(バインドマウント)
- docker 内に持つ場合(ボリュームマウント)

### docker-compose その他

- environment で環境変数を指定することができる。だが DB の Password などは渡さないように
- depends_on:で先に起動するものを指定できる。
- links:を使うと、

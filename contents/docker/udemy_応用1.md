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
`/var/lib/docker`がdockerオブジェクトで埋まっていく。
そこの容量が足りないとrunできない。
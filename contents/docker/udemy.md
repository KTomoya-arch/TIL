# カメさんの Docker udemy 講座

## Linux の歴史

- Unix をベースに作られている
- Unix をもとに Linux をリーナストーバルズが 0 ベースで作った
- Mac は Unix ベースの OS、Linux は Unix をもとに作られている

## Linux へのコマンド

Linux には Kernel という核があるがその周りに Shell(殻,bash,zsh,sh)がありシェルを通じて Kernel へ命令を伝達する。
Shell を起動するために使用するツール(app)がターミナルである。

## Docker コンテナ

- Docker のコンテナは Docker イメージから作成される。
- Docker イメージは Dockerfile から作成される。設計図のようなものである。
- DockerHub にはたくさんのイメージが保存されている。

- `$ docker run images`でコンテナを起動できる。
- `$ docker ps`はアクティブなコンテナを確認することができる。

### hello-world コンテナを起動してみる

hello-world コンテナにはテキストを出力する(下記)のようなプログラムになっていて、
それを実行して、Exited(終了)した。

```
~/D/TIL ❯❯❯ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

- つまり、コンテナはずっと起動しておくというようなパターンもあれば使っては終了してという使われ方をすることもある。

- コンテナ起動 ⇨ コンテナ作成 ⇨ コンテナ内のプログラム実行 ⇨ コンテナが終了

### docker run -it ubuntu bash を実行してみる

`$ docker run -it ubuntu bash`の `it`はおまじないであり、`bash`は対象となるプログラムである。
ubuntu のコンテナを起動し、bash というプログラムを実行させるという意味を表す。
sh にすれば当然、sh が実行される。
ubuntu はイメージの名前を表している。
イメージはローカルに存在しなければ、勝手に pull してくれる。

#### 結果

```bash
~/D/TIL ❯❯❯ docker run -it ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
79d0ea7dc1a8: Pull complete
Digest: sha256:dfd64a3b4296d8c9b62aa3309984f8620b98d87e47492599ee20739e8eb54fbf
Status: Downloaded newer image for ubuntu:latest
root@2d5fd2693916:/#
```

### docker コンテナが起動されるとどうなるのか

そもそも docker イメージを pull した際に複数のイメージが折り重なってできている。
それらの上に、新しく layer を追加するということが行われる。
その新しいイメージは書き込みが可能な layer となっている。

#### 新しくイメージlayerが追加されると何が嬉しいのか？
1つのイメージから複数のコンテナを作成する場合、例えば、ubuntuからコンテナAとコンテナBを作成する場合、
AとBで新しいlayerはそれぞれ別で作られるが、ubuntuのベースイメージは共有される。

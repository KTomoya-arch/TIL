# さらにコンテナというものについてフォーカスする

## docker run = create + start

docker run はローカルでイメージを探し、なければ docker hub に取りに行き、コンテナ化をする。
docker run コマンドは厳密にいうと、docker のコンテナを create し、start している。
docker run hello-world を行うと、コンテナを立てて、デフォルトのコマンドを実行した。(下記の結果になった)

```
docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
70f5ac315c5a: Pull complete
Digest: sha256:fc6cf906cbfa013e80938cdf0bb199fbdbb86d6e3e013783e5a766f50f5dbce0
Status: Downloaded newer image for hello-world:latest

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

確認をすると、コンテナは`Exited`となっている。

```
~/D/T/c/docker ❯❯❯ docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED              STATUS                          PORTS     NAMES
b0f395956cd5   hello-world    "/hello"   About a minute ago   Exited (0) About a minute ago             loving_volhard
```

### create と start は何だ？

### `$ docker create <image>`

create を実行すると下記のようになる。
STATUS は Created でコンテナを作っただけで、コマンドは実行されない。

```
~/D/T/c/docker ❯❯❯ docker create hello-world
d509dd1655a6b33619dbf48bd55c2536d32a0c74171ed23740aa8726908647b1

~/D/T/c/docker ❯❯❯ docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED          STATUS                     PORTS     NAMES
d509dd1655a6   hello-world    "/hello"   2 seconds ago    Created                              affectionate_shockley
```

### `$ docker start <container_id>`

start を実行すると下記のようになる。
Exited になっている。
あれ？create + start は run ならば、docker run した時と同様に HelloWorld のテキストが出力されるのでは・・・？
STATUS も Up じゃないし・・・。
だが、デフォルトの helloworld のコマンドは実行されている。
docker start ではデフォルトのコマンドが実行された結果は見えず、オプションをつける必要がある。(`オプション -a`(-atach の略))

```
~/D/T/c/docker ❯❯❯ docker start d509dd1655a6
d509dd1655a6

~/D/T/c/docker ❯❯❯ docker ps -a
CONTAINER ID   IMAGE          COMMAND    CREATED         STATUS                      PORTS     NAMES
d509dd1655a6   hello-world    "/hello"   3 minutes ago   Exited (0) 22 seconds ago             affectionate_shockley

~/D/TIL ❯❯❯ docker start -a d509dd1655a6

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

### コマンドの上書き

`$ docker run -it ubuntu bash`
普通に docker run をすると、イメージのデフォルトコマンドが実行される。
だが、`$ docker run <image> <command>`で上書きをすることができる。
（例）
`$ docker run -it ubuntu bash`

### まとめ

- docker run = create + start を一緒にやっている
- docker create はコンテナを作るだけでステータスは Created
- docker start はコンテナを起動させてデフォルトのコマンドを実行する、そしてコンテナを抜ける
- 結果を見たい場合は、docker start に -a のオプションをつける。
- docker ps -a で COMMAND 列にあるものが、デフォルトのコマンドである。hello-world の場合は、"/hello"であり/hello が実行されている。
- このデフォルトのコマンドは上書きをすることが可能である。(docker exec -it /bash で上がいている)

## -it は何だ？

`$ docker run -it ubuntu bash`
とうとう、-it が何かについて触れる。

-i: インプット可能 <br>
-t: 表示が綺麗になる

### `$ docker run -t ubuntu bash`

-i 無しで実行するとどうなるか・・・？
下記のようになる<br>
ubuntu で ls を実行しているので本来ならばルートディレクトリのファイル、ディレクトリ名が表示されるはずだが何も表示されない。
ls がコンテナに通じていない、ホストからコンテナへ入力が届いていないのである。
Linux には 3 つのチャネルが存在し、stdin 、stdout、 stderr が存在する。
キーボードの入力が stdin を通じてプログラムへ伝達され、stdout や stderr を通じて、ディスプレイへ結果が表示される。
-i は、ホストからコンテナへのチャネルを開くためのオプションである。なのでつけないと開かない。

```
~/D/TIL ❯❯❯ docker run -t ubuntu bash
root@803364585a54:/#


^C^C
root@803364585a54:/# ^C
root@803364585a54:/# ls

```

### `$ docker run -i ubuntu bash`

-t を除いて実行すると下記のようになる。
綺麗に表示がされない上に tab 補完も効かない。

```
~/D/TIL ❯❯❯ docker run -i ubuntu bash
ls
bin
boot
dev
etc
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

## 削除、停止、全削除

### 削除

停止されているコンテナを削除することができる。
STATUS が Up になっているものは削除できない。
`$ docker rm <コンテナID> `

### 停止

`$ docker stop <コンテナID>`

### 全削除

`$ docker system prune `
停止しているコンテナを全て削除することができる。
dangling images とは Dockerfile から作られたばかりの Docker image のことをさす。
タグ付けされていない none になっているイメージのことを指す。

```
~/D/TIL ❯❯❯ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N]
```

## コンテナのファイルシステム

コンテナのファイルシステムというのは、ホストのファイルシステム、他のコンテナのファイルシステムからも独立している。

### `$ docker run --name <name> <image>`

コンテナに名前をつける。どういう時？

- 起動させ続けるコンテナを立てる時
- 共有サーバを使用すると時
- 他プログラムから使用する時

同じ名前のコンテナを複数作ることはできないので名前を変えるか、片方を削除する必要がある。

### detached mode と foreground mode

- detached mode<br>
  `$ docker run -d <image>`で起動し、コンテナを起動後に detach する(バックグラウンドで動かす)

- (short-term) foreground mode<br>
  `$ docker run --rm <image>`
  コンテナを Exit 後に削除する(一回きりのコンテナ)
  hello-world のコンテナとか一度きりで使用して捨てるようなコンテナには--rm をつけるのがおすすめ。
  そうでないと、docker ps -a をした時にコンテナが溜まっていく。

## おさらい

- run = create+start である
- デフォルトのコマンドを上書きするには、$ docker run <image> <command>
- -it オプション、i はインタラクティブであり、入力チャネル(stdin)を繋ぐ。 -t は綺麗に出力をしたり、tab 補完を効かせる
- コンテナの削除 docker rm , docker system prune
- コンテナのファイルシステムは独立している
- コンテナ名を指定する docker run --name <name> <image>
- detached & foreground(docker run -d <image>、 docker run --rm <image>)

## dockerfile に関して

`$ docker build .`
Dockerfile から Docker イメージを作る →build
ビルドしたばかりのイメージをダングリング(ぶら下がっているという意味)イメージという。

`$ docker build -t new-ubuntu:latest .`
none になっているのがうざいので t でつける。

Dockefile というのは Docker イメージの設計書であり、dockerfile から docker イメージを作成する。

### alpine

alpine は非常に軽い

### dockerfile を作る

docker イメージに RUN コマンドで色々操作できるがその分レイヤーが一つずつ追加されてしまい、容量が大きくなってしまう
これも課題であり、どうやって軽くするかは重要である

### Ubuntu

Ubuntu では apt-get(apt)コマンドでパッケージの管理を行う

- apt-get update で新しいパッケージのリストを取得する
- apt-get install <package> パッケージをインストールする

### dockerfile を書くベストプラクティス

- image レイヤは最小限にする
- Layer を作成するのは`RUN, COPY, ADD`の 3 つである。
- コマンドを&&で繋げる
- バックスラッシュ(\)で改行を行う

#### Dockerfile を作るときはキャッシュをうまく活用しよう！

apt-get とか apt-get update は時間がかかる。

```
RUN apt-get update && apt-get install -y \
    curl \
    nginx
```

上記の dockerfile があった際に、さらに `cvs`もインストールしたいとなった場合、
中に、cvs を追加すると RUN まるごとレイヤを作りインストールも行うので非常に時間がかかる。
なので dockerfile をメンテナンス、作っていく家庭では、RUN を追加して
以前の install したものに関してはキャッシュを使用して行うようにする。

```
FROM ubuntu:latest
RUN apt-get update && apt-get install -y \
    curl \
    nginx
RUN apt-get install -y cvs
```

下記のようにキャッシュを利用してくれる。

```
 => CACHED [2/3] RUN apt-get update && apt-get install -y     curl     nginx                                                                                                  0.0s
 => [3/3] RUN apt-get install -y cvs
```

もちろん、完成品となる dockerfile については RUN の数は少ければ少ないほど軽量となるため良い。

#### CMD コマンド

FROM,RUN,CMD を基本 3 つ覚えておけば良い。

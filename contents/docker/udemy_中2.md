## docker build の詳細

`$ docker build .` この場合、 . がフォルダになる。<br>
docker build をすると、指定したフォルダ(.)を docker デーモンに渡している。
このフォルダのことをビルドコンテキストという。
docker build したときは指定したフォルダをビルドコンテキストとして docker デーモンに渡している。

- なぜ docker bulid というコマンドはビルドコンテキストを使用してイメージを作っているのだろうか？<br>
  ビルドコンテキストにあるファイルをビルドの際に使用することができるからである。
  具体的にそれを COPY,ADD などのコマンドで使用したりしている。(COPY,ADD,ENTRYPOINT,ENV,WORKDIR など)

ビルドコンテキスト(対象となるフォルダ)には不要なファイルは置かない。

### docker instruction

ADD や COPY というコマンドを使用すると、ビルドコンテキストの中にあるファイルを image に持っていくことができる。
なのでただビルドコンテキストにファイルを置いてあるだけだと、image の中に持っていくことはできないが
docker デーモンに対しては送ってはいる。

### ADD コマンドと COPY コマンド

何が違う？使い分けはどうすればいい？
⇨ 前提知識として、ADD は COPY に比べて機能が多い。ちょっと上級者向けである。

- 単純にファイルやフォルダをコピーする場合は COPY を使う
- tar の圧縮ファイルをコピーして解凍したいときは ADD を使う

### CMD の復習

CMD はコンテナのデフォルトのコマンドをセットする。
CMD["ls", "-l"]

### ENTRYPOINT

- ENTRYPOINT は、docker run 時にデフォルトのコマンドを上書きすることはできない。
- ENTRYPOINT がある場合は、CMD は["param1", "param2"]のような形をとる。つまり ENTRYPOINT で指定したコマンドの引数を書く。
- run 時に上書きが可能な部分は CMD の引数の部分のみである。
- コンテナをコマンドのようにして使いたい時に使用する

### ENV

環境変数を設定する
下記のように書けば ENV を定義できる。

```
FROM ubuntu:latest
ENV DEBIAN_FRONTEND=noninteractive
ENV UNKO hogehoge
```

### WORKDIR

docker instruction の実行ディレクトリを変更する。

```
RUN mkdir sample_folder
RUN cd sample_folder
RUN touch sample_file
```

上記の RUN で指定したコマンドは、すべてルートディレクトリで実行される。
cd sample_folder の後 touch sample_file も当然ルートディレクトリに作成される。
実行ディレクトリを変えることができる。

## ホストとコンテナのファイル共有

`$ docker run -it -v hostのディレクトリ名:コンテナのディレクトリ名`
コンテナは何も指定をしないと、ルートユーザーで起動される
コンテナとホストマシンを共有した際に、ルートユーザーで起動されてしまうと
コンテナのディレクトリを操作した際に、ホスト側(共有先)を自由に操作できてしまうため、
セキュリティ的に課題がある。なのでユーザーを指定して、コンテナを起動する。

### cpu や memory の指定を行う

--cpus <ofCPUs> --memory <byte>

### port の指定

-p <host_port>:<container_port>

### コンテナの詳細を表示する

`$ docker inspect`
大量にデータが表示されるので
grep -i と併用すると良い。

`$ docker inspect <container_id> | grep -i <something>`

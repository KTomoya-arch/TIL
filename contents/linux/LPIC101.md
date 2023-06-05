# あずき本学びメモ

- SysVinit<br>
  元々 UNIX 系では起動手順で SysVinit が主流出会ったが最近は systemd が主流になっている
  sysVinit では Linux で最初に実行されるプロセスである init が/etc/inittab のファイルに従い必要となるサービスを順次起動していく
  - init が/etc/inittab ファイルを読み込む
  - init が/etc/rc.sysinit スクリプトを読み込む
  - init が/etc/rc スクリプトを読み込む...
    しかし最近では/etc/inittab ファイルは存在しないことも多い
    SysV init はあらかじめ決められた順にサービスが起動していくので(同期的)、現在は systemd や Upstart といった仕組みに切り替わっている。
- Upstart<br>
  Upstart はイベント駆動型でランベルの変更などをトリガーとして指定された処理を実行するような感じになっているが今は systemd への移行が進んでいる。ランレベル＝ユーザーモード？

SysVinit では/etc/init.d ディレクトリに用意された起動スクリプトが使用される。
起動スクリプトは主にシステムサービスや各種サービスを起動したり、再起動、終了する際に使用される
linux 起動の最初のプロセスとして init が実行され、指定されたランレベルで起動される、デフォルトのランレベルが/etc/inittab に記述されている

- systemd
  linux のシステムやサービスをマネジメントするようなソフトウェアである。
  init のシステム linux のシステムを管理する
  systemd を採用しているシステムでは init プロセスの代わりに systemd プロセスが起動し、各種サービスを管理する。systemd では複数のデーモンが連携して動作する。
  デーモンというのはメモリ上に常駐してシステムサービスやサーバーサービスを提供するプロセスである
  systemd ではシステムの起動処理は多数の Unit と呼ばれる処理単位に分かれている。
  例えば httpd.service は httpd サービスの起動に使われる Unit である。systemd では serviceA や serviceB の起動の順序関係や依存関係を処理できる、また並列的かつ必要なサービスのみが起動されるので sysVinit に比べて systemd の方が時短である
  linux のサービスなどをどんどん立ち上げる役割としても systemd がになっていたりする
  sysVinit は直列に立ち上げる、systemd は並列に実行できる

## systemd の立ち上げプロセスで自分のアプリケーションを起動する？

systemd はプレーンテキストで設定を行う
設定を書き込むプレーンテキストのファイル 1 つ 1 つのことを systemd の文脈では Unit ファイルという(サービス、アプリケーションの１単位、中身は.ini 的な設定ファイルフォーマットに従っておりいろいろなタイプが存在する)

- .service<br>
  サービスデーモン(サービスを管理する)
- .timer<br>
  繰り返ししたいアプリケーションの設定に使う
- .device
- .mount
- .automount
- .socket
- .target
  ...

## systemd unit の使い方

systemctl を使用して systemd units を扱う

### Use systemctl to operate systemd units

- analyze system status
- Use systemd units
- Manage power(電源管理)

systemctl の commands

- status
- list-units
- start UNIT
- stop UNIT

### systemd の unit files はどこ？

下記の/etc...の方が優先的に読み込まれる。
yum やコマンドで install した際の unit ファイルを変更したい時は/user/lib の unit ファイルを/etc 配下へコピーして編集するのが一般的である。

- /usr/lib/systemd/system/(パッケージをインストールした際に提供される)
- /etc/systemd/system(ユーザー管理である,ユーザー定義である)

### ユニットファイルの構造

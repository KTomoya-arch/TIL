## Docker を使ってデータ解析環境を構築する

<補足>

```
M1チップのMacの場合，最新のAnacondaのバージョン(https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh)において，
次レクチャーにて
「$sh /opt/Anaconda3-2022.05-Linux-x86_64.sh -b -p /opt/anaconda3」
を実行すると
「 Could not open '/lib64/ld-linux-x86-64.so.2': No such file or directory」
のようにエラーが出ることが確認できています．
前レクチャーの最後のdocker buildの際に「--platform linux/amd64」オプションをつけることで問題なくAnacondaをインストールすることができることが確認できています．

(コマンドは以下の様になります
「docker build --platform linux/amd64 .」
また，お使いのDocker desktopのバージョンによっては，--platformオプションがうまく認識されず，エラーになる可能性があるようです．その場合は，
「docker buildx build --platform linux/amd64 .」
のようにbuildxを使ってみてください．buildxでエラーが出る場合は，Docker Desktopのバージョンを最新にするなり，こちらのドキュメント(https://docs.docker.com/buildx/working-with-buildx/)を参考にインストールを試みてください．(こちらで動作確認ができているDocker Destktopのバージョンは4.10.0です
M1チップ登場とDocker側の開発状況によって講座の内容がうまくいかないことが報告されていますが，こちらの方でなるべく最新情報を更新していきますのでよろしくお願いします．

また，講座でサポートしているバージョン以外のものをお使いの場合はこちらでのサポートが限定的になることをご了承ください！
```

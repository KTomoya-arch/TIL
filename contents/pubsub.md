# Cloud Pub/Sub に関するメモ

## Pub/Sub について調べようと思った経緯

- 自社のアーキテクチャにおいて Cloud Pub/Sub を使用している。
- 具体的に何に使用されているのか、何故 Pub/Sub を使用しているのかなどを言語化できていない。
- また下記の状況下でエラーが発生したがエラーログ、pub/sub で使用されるメッセージが確認できず根本的な原因究明に至れなかった。(確認方法や手順がチームで確立されていない。)

```
Cloud Scheduler(日次バッチ) → Pub/Sub dailyTopics → salaryServiceDailySubscription(salary の rest endpoint) & SurveyServiceDailySubscription(survey の rest endpoint)⇨ notification Topic → notification-service endpoint

```

## Cloud Pub/Sub とは何？

リアルタイムデータやイベントデータの取り込みを行うためのメッセージングサービスである。
分析イベントの取り込みやストリーミングなどを効率的に行うことができる。

- ストリーミング<br>
  データを受信しながら同時に随時再生をしていく方式

Google のサーバレスなフルマネージドサービスである。
pubsub はメッセージの送信者である Publisher がメッセージを受け取る人である Subscriber にメッセージを送信するためのサービスであり、
pubsub を活用すると subscriber が複数のメッセージを同時に受信したり、publisher が複数の subscriber に対して同時にメッセージを配信したりすることができる。
![](https://i.gyazo.com/75495afd3d24029e19a5963d4c46dfcd.png)

pubsub ではトピックとサブスクリプションを作る必要があり、トピックはメッセージの種類ごとに作成する。

### Pub/Sub Lite？

Pub/Sub と異なり Pub/Sub の簡易版である。違いは単一ゾーンでのサービスとなっている。信頼性は劣るがコストを抑えながら利用ができる点がメリット。

# BFF とは？

NALYSYS では FE と BE の間に BFF(backend for frontend)が存在している。

## 背景<br>

クライアント端末が増加したことによるフロントエンド側のソースコード複雑化である。(メッセージやコンテンツの出し分けをクライアントアプリに合わせて実装しようとすると複雑化するという話みたい。)
そのような背景で複数のバックエンドから情報を取得し適切な形に整形するなどのロジックを持つサーバーを導入するということでフロントエンド側を UI/UX に注力するようにしたのが BFF 登場の経緯である。

## BFF のメリット

### API の集約

シンプルにクライアントは BFF に対してのみ、API を投げれば良い。
NALYSYS では、employee-service,account-service,personal-service,survey-service,labor-service,labor-ea-service,mynumber-service...など複数の MS によって構成されており、API の呼び出し先が複雑化していると言える。それを engine-frontend からのリクエストは api.${service}.${env}.worx-hr.com へ投げるだけで良くなっておりフロントエンドからみたバックエンドは非常にシンプルになっている。

### バックエンド差異の吸収

バックエンドが grpc、rest、websocket などさまざまな api 仕様でもその差異を BFF が吸収できるというメリットがある。
(NALYSYS では GraphQL、RESTful)

## 
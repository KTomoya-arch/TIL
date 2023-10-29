## 第１部

- SRE とは何？

- SRE が IT 業界における従来型の実践手法とは何が異なるのか？-

シスアド(システム管理者を立てる)場合、つまり運用と開発を分断するモデルはコストがかかってしまう。
直接的なコストとしては変更管理やイベント処理を手作業に頼るチームがサービスを稼働させる場合
サービスのスケールやトラフィックの増大に伴いコストが嵩んでしまう。つまりチームを大きくしなければならなくなる。

間接的なコストはバックグラウンド、スキルセット、動機づけが全く異なるし技術的な解決策のリスクや可能性についても異なる推定をする。
状況説明に異なる言葉を使用する。それゆえに開発と運用はプロダクト開発においてしばしば衝突する。

Google の SRE の採用に対するアプローチの結果、手作業でタスクをこなすことにすぐ飽きてしまい、それまで手作業で行っていたことをたとえそれが複雑なもので合っても、代替してくれるソフトウェアを書くのに必要なスキルセットを持つエンジニアたちのチームができた

SRE はアカデミックで知的なバックグラウンドを開発組織と共有することになり、運用チームが行なってきたことをソフトウェアの専門性を持つ
エンジニアが行い、エンジニアが人手による管理を自動化するソフトウェアを設計し実装する能力を持ちそれを厭わないということから成り立っている。SRE はエンジニアリングに注力せよ。

Google では全ての SRE に対して、チケット・オンコール・手作業といった運用の業務の合計を 50%以下にするよう上限を設定した。
残りの 50%の時間を実際に開発を行うために使わなければならないというものである。どう実行する？

- SRE がどう時間を使っているかの計測し習慣を変える
  そうすること運用と開発の作業バランスを意識して維持することでクリエイティブで自律的なエンジニアリングに携わる SRE を確保しつつ一方でサービスを動作させる運用面から収集された知見を維持できる。

  1.3 SRE の信条
  SRE はサービスの可用性、レイテンシ、パフォーマンス、効率性、変更管理、モニタリング、緊急対応、キャパシティプランニングに責任を負う。

  1.3.1 エンジニアリングへの継続的な注力
  Google では SRE が運用作業にかける時間を 50%に設定し、残りはコーディングスキルを使うプロジェクトの作業に充てることが求められている。
  そのため、SRE が行う運用作業の量をモニタリングし、超過した運用業務をプロダクト開発チームへ差し戻している。つまりバグとチケットを
  開発チームのマネージャーに割り当て直したり、開発者をオンコールのページャーローテーションに組み込んだりといったことにより実現されている。このアプローチが上手く行くのは組織全体、すなわち SRE と開発者がこの仕組みが存在する理由を等しく理解し、運用負荷のオーバーフローが起こらない、プロダクトが課題な運用負荷を生じさせないという目標を支持しているからである。

  1.3.2 サービスの SLO を下回ることなく、変化の速度の最大化を追求する。
  エラーバジェットは 100%を信頼性の目標とすることおはいかなる場合でも間違っている。100%と 99.999%の違いってなんですか？
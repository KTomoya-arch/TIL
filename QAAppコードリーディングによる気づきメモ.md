# QAAppコードリーディングによる発見メモ
- .rspec
テストのためのフレームワーク
- rubocop-rails:
コーディング規約に準拠しているかチェックするためのgem
- layoutメソッドの使用：
layout 'ファイル名'とすることでlayout/application.htmlではなくlayout/'ファイル名'が読み込まれる
- erbとslimではモダンフロントへの移行なども考えると.erbのが優？
- modelにscopeを定義することで複雑なクエリなどをシンプルに定義し利用することが可能
```
scope :related_to_question, ->(question) { joins(:answers).where(answers: { question_id: question.id }) }
```


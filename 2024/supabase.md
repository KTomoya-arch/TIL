# supabase

https://tech.smartcamp.co.jp/entry/try-supabase
オープンソースな Firebase の代替

## サービス

- PostgreSQL ベースのデータベース
- 認証<br>
  (SNS 認証対応しているサービスで Firebase 同様)
- ファイルストレージ<br>
- API の自動生成<br>
  管理画面からテーブルやカラムを追加した際に、自動的に API へのルーティングを生やしてくれる機能
  公式ドキュメントにも例としてありますが、ToDo テーブルを作った際、ToDo に対する GET、POST、PATCH、DELETE のリクエストを走らせることができるようになります。
  下にドキュメントに記載されている例文を載せておきます。

```
// Initialize the JS client
import { createClient } from '@supabase/supabase-js'
const supabase = createClient([SUPABASE_URL], [SUPABASE_ANON_KEY])

// Make a request
let { data: todos, error } = await supabase
  .from('todos')
  .select('*')
```

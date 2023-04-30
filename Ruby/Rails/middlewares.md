### Rack

> Rackは、RubyのWebアプリケーションに対して、モジュール化された最小限のインターフェイスを提供して、インターフェイスを広範囲に使えるようにします。RackはHTTPリクエストとレスポンスを可能なかぎり簡単な方法でラッピングすることで、Webサーバー、Webフレームワーク、その間に位置するソフトウェア（ミドルウェアと呼ばれています）のAPIを1つのメソッド呼び出しの形にまとめます。


https://github.com/rack/rack

https://railsguides.jp/rails_on_rack.html

> Rackはどこで使われ、なぜ必要か
Rackはアプリサーバーとアプリ(Webフレームワーク)間のデータのやり取りに利用します。
PumaやUnicornといったアプリサーバーは、特定のアプリ専用(たとえばRails専用)というわけではなく、RackのプロトコルにしたがっているWebフレームワークであれば何でも利用できます。

https://qiita.com/nishio-dens/items/e293f15856d849d3862b

https://zenn.dev/igaiga/books/rails-practice-note/viewer/rack_middleware_and_rack

https://logmi.jp/tech/articles/322627

https://docs.newrelic.com/jp/docs/apm/agents/ruby-agent/instrumented-gems/rack-middlewares/

-----

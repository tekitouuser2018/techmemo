https://cloud.google.com/spanner?hl=ja

> Cloud Spanner は、セカンダリ インデックス、強整合性、スキーマ、SQL などのリレーショナル セマンティクスを組み合わせ、単一の簡単なソリューションで 99.999% の可用性を提供する、スケーラビリティの高いデータベースです。したがって、リレーショナル ワークロードと非リレーショナル ワークロードの両方に適しています。
>
> Cloud Spanner では、同一の豊富な機能セットに対して GoogleSQL と PostgreSQL の 2 つの ANSI ベースの SQL 言語を使用できます。GoogleSQL は、BigQuery と構文を共有しているので、チームのデータ管理のワークフローを標準化できます。PostgreSQL Interface により、すでに PostgreSQL を理解しているチームにとって馴染みやすく、他の PostgreSQL 環境へのスキーマやクエリのポータビリティも得られます。
>
> Spanner への移行は、ソース データベース、データサイズ、ダウンタイム要件、アプリケーション コードの複雑さ、シャーディング スキーマ、カスタム関数または変換、フェイルオーバーとレプリケーション戦略など、多くの要因によって大きく異なる場合があります。スキーマとデータの移行用には HarbourBridge のようなオープンソース ツール、評価用には　migVisor などのサードパーティ ツールをおすすめします。
>
> Spanner はフルマネージドのデータベースなので、インフラストラクチャの包括的な管理機能が自動的に提供されますが、ワークロードによっては、アプリケーション固有の管理アクションが必要になる場合があります。本番環境を常にスムーズに稼働させるには、適切なアラートとモニタリングをセットアップし、注意深く監視する必要があります。時間の経過とともにトラフィックが有機的に増加した場合やピーク　トラフィックが想定される場合に、どう対処すべきかや、アプリケーションのバグに起因するデータの破損を処理する方法を理解する必要があります。さらに、パフォーマンスの問題をトラブルシューティングする方法と、レイテンシ増加の原因となるコンポーネントを把握する方法を理解することも重要です。


https://recruit.gmo.jp/engineer/jisedai/blog/cloud-spanner-startguide/

https://cloud-ace.jp/column/detail387/

https://developers.cyberagent.co.jp/blog/archives/39843/

#### スキーマ

・HotSpot発生を回避するため、主キーはUUID v4などランダムな値を使用する

https://cloud.google.com/spanner/docs/schema-and-data-model?hl=ja

#### Split

https://zenn.dev/facengineer/articles/bca8790087b0e4


#### インターリーブ

・Cloud Spannerでは、主にインターリーブを使用して親子関係を構築する

https://qiita.com/pa_pa_geno/items/c5fc5ea0d5daf415080d

https://medium.com/google-cloud-jp/cloud-spanner-%E3%81%A7%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%AA%E3%83%BC%E3%83%96%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E3%82%92%E9%AB%98%E9%80%9F%E3%81%AB%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B-2a955b061d3




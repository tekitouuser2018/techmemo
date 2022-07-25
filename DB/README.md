**MySQl(InnoDB) - マルチスレッド。ネイティブで UUID 型をサポートしていない。**

**PostgreSQL - マルチプロセス。基本はUUID v4**

**MVCC（多版型同時実行制御）**<br/> 
> 多くのデータベースシステムでは、同時実行制御のためにロック機構を使用していますが、PostgreSQLではデータ整合性の維持に多版方式を使用しています。つまり、データベースへの問い合わせ実行の際、各トランザクションは処理の基礎となっているデータの現在の状態を関知せず、現在から遡ったある時点におけるスナップショット（データベースバージョン）を参照する、というものです。これは、並行する（別の）トランザクションが同じ行を更新することによって引き起こる、整合性を欠いたデータの参照からトランザクションを保護し、個々のデータベースセッションに対してトランザクションの隔離を提供するものです。
> 多版方式とロック方式との最大の相違点は、MVCCでは問い合わせ（読み込み）ロックの獲得と、書き込みロックの獲得が競合しないことです。したがって、読み込みは書き込みを絶対にブロックしませんし、書き込みも読み込みをブロックすることがありません。
https://www.postgresql.jp/document/7.2/user/mvcc.html<br/> 
https://devcenter.heroku.com/ja/articles/postgresql-concurrency

**MVCC の欠点**<br/>
> 可視である行のセットはトランザクションごとに異なるため、Postgres では、古くなっている可能性があるレコードを維持する必要があります。UPDATE​ が実際には新しい行を作成するのはこれが理由であり、DELETE​ が実際には​行を削除せず、行を削除済みとマークして XID 値を適切に設定するだけである理由も同じです。トランザクションが完了すると、将来のどのトランザクションからも可視になる可能性がない行がデータベースに残ることになります。これらはデッドロー (dead row) と呼ばれます。MVCC に由来するもう 1 つの問題として、トランザクション ID はただ大きくなり続けることしかできません。32 ビットであり、サポートできるのは 約 40 億トランザクション “のみ"です。最大値に達すると、XID は最小値に戻って (ラップアラウンドして) ゼロから再開します。突然、すべての​行が将来のトランザクションにあるかのように見え、新しいトランザクションはそれらの行への可視性を得られなくなります。
デッドローとトランザクション XID ラップアラウンドの問題はどちらも VACUUM​ によって解決されます。これは定期的に必要なメンテナンスですが、幸いにも Postgres には、設定可能な頻度で実行される auto_vacuum デーモンが付属しています。デプロイが異なれば必要なバキューム頻度も異なるため、これを注視することは重要です。VACUUM​ の実際の動作について詳しくは、Postgres のドキュメント​を参照してください。Heroku での処理​方法も参照してください。<br/>
https://devcenter.heroku.com/ja/articles/postgresql-concurrency#disadvantages-of-mvcc

トランザクション分離レベル<br/> 
https://qiita.com/song_ss/items/38e514b05e9dabae3bdb<br/> 
https://qiita.com/PruneMazui/items/4135fcf7621869726b4b

PostgreSQLのread committed時におけるUPDATEの挙動について（ファジーリード）<br/>
https://soudai.hatenablog.com/entry/2022/07/03/223915<br/>
https://qiita.com/mpyw/items/14925c499b689a0cbc59

**Postgres と MySQL における id, created_at, updated_at に関するベストプラクティス**
> 一般的に採用されやすいプライマリキー用の値として，以下を考える。<br/>
連番整数<br/>
MySQL では AUTO_INCREMENT， Postgres では IDENTITY や SERIAL と呼ばれるもの<br/>
UUID v1: ハードウェアごとにユニークな単調増加値<br/>
UUID v4: ランダム値<br/>
UUID v7（ドラフト）: 単調増加であるタイムスタンプとランダム値の複合<br/>
https://zenn.dev/mpyw/articles/rdb-ids-and-timestamps-best-practices

PostgreSQLのガベージコレクション(vacuum)<br/>
https://www.postgresql.jp/docs/9.4/sql-vacuum.html<br/>
https://masahikosawada.github.io/2021/12/22/MVCC-and-GC-in-PostgreSQL/

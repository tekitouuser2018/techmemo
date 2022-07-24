**MySQl(InnoDB) - マルチスレッド**

**PostgreSQL - マルチプロセス**

**MVCC（多版型同時実行制御）**<br/> 
> 多くのデータベースシステムでは、同時実行制御のためにロック機構を使用していますが、PostgreSQLではデータ整合性の維持に多版方式を使用しています。つまり、データベースへの問い合わせ実行の際、各トランザクションは処理の基礎となっているデータの現在の状態を関知せず、現在から遡ったある時点におけるスナップショット（データベースバージョン）を参照する、というものです。これは、並行する（別の）トランザクションが同じ行を更新することによって引き起こる、整合性を欠いたデータの参照からトランザクションを保護し、個々のデータベースセッションに対してトランザクションの隔離を提供するものです。
> 多版方式とロック方式との最大の相違点は、MVCCでは問い合わせ（読み込み）ロックの獲得と、書き込みロックの獲得が競合しないことです。したがって、読み込みは書き込みを絶対にブロックしませんし、書き込みも読み込みをブロックすることがありません。
https://www.postgresql.jp/document/7.2/user/mvcc.html<br/> 
https://devcenter.heroku.com/ja/articles/postgresql-concurrency


トランザクション分離レベル<br/> 
https://qiita.com/song_ss/items/38e514b05e9dabae3bdb<br/> 
https://qiita.com/PruneMazui/items/4135fcf7621869726b4b

PostgreSQLのread committed時におけるUPDATEの挙動について（ファジーリード）<br/>
https://soudai.hatenablog.com/entry/2022/07/03/223915<br/>
https://qiita.com/mpyw/items/14925c499b689a0cbc59

PostgreSQLのガベージコレクション(vacuum)<br/>
https://www.postgresql.jp/docs/9.4/sql-vacuum.html<br/>
https://masahikosawada.github.io/2021/12/22/MVCC-and-GC-in-PostgreSQL/

### フリーWi-Fiを使ったら秘密情報を抜かれる経路にはどのようなものがあるか

個人的感想： 一般の利用者が見破ることが困難な攻撃もあり、また考慮すべき点が多く個人の情報リテラシーの差を考えると、組織内のルールとしては使用しないとする方が良い。

https://qiita.com/ockeghem/items/c6a3602d2c2409f89fbb

----
### 「転職ドラフト」上において、ご利用に関する一部のデータがHTMLソース上にて第三者による閲覧が可能な状態であったことが、2023年5月31日に判明

https://job-draft.jp/articles/580

防止策、対応などの個人的見解

- 認可：本人と第三者でアクセス認可の管理を実施する。例：railsのpunditを使用
- API設計：同じ項目の情報を参照する場合でも本人と第三者でアクセスできるAPIを分割する
- JSONアイテムの整理：サーバーからフロントへ渡すJSONの内容を整理する。例：railsのjbuilderを使用

----

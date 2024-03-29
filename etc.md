### 勉強会

#### 【10X×リンクアンドモチベーション】どう向き合う？ChatGPT時代のエンジニア組織づくり

https://findy.connpass.com/event/279910/?utm_campaign=event_message_to_selected_participant&utm_source=notifications&utm_medium=email&utm_content=title_link

「印象に残った点、気になった点」
- 10xはドメイン毎のチーム編成、リンクアンドモチベーションは横串のチーム編成

  → 自分なりの考え
  
  - 横串チームは小規模な組織に向いている。ドメイン毎の縦割りチームはある程度大きな組織に向いている。
  - 縦割りと横割りは性質としてトレードオフの関係にあるため両方の性質を併せ持つことはできない。もし、両方の性質が組織にとって必要であれば別々のチーム編成が必要となるはず。
  - これはITシステム設計と管理と同じように考えることができる。モノリシックなシステムは横断的な管理となり、マイクロサービスは縦割りとなる。さらにマイクロサービスの設計や管理には専門的な知識や工夫が必要となる。

- 仕様漏れというものは毎回発生する。ChatGPTを仕様漏れの確認の壁打ち相手に使用できる(10x)

  → 自分なりの考え
  
  - ChatGPTは中央値を出すのに適しているため(10x自身も発言していた)、コーナーケースや特殊ケースには対応できない可能性が高い。そのため、仕様確認の際には特に特殊ケースに対して注意する必要がある。

----

#### DX内製化の裏側に迫る〜東急とパイオニアが目指すエンジニア組織〜

https://findy.connpass.com/event/279575/?utm_campaign=event_message_to_selected_participant&utm_source=notifications&utm_medium=email&utm_content=title_link

「印象に残った点、気になった点」
- マネージャーが存在せず各シニアエンジニアが自発的に動いているような状況で、属人化しないようドキュメントなど残してもらうようにしている。
- 内製化について、まず小さく始める

----

#### それってPM業務？みんなの組織のPM担当範囲を聞いてみよう【開発PM勉強会 vol.19】

https://peer-quest.connpass.com/event/278275/?utm_campaign=event_message_to_selected_participant&utm_source=notifications&utm_medium=email&utm_content=title_link

「印象に残った点、気になった点」
- 現場によって役割は全く異なってくる。
- やらないことを決める。やらないことリストの作成と情報共有。デシジョンツリーの使用。

  → 結果: ミーティングの時間を大幅に削減。開発やリリースに余裕を持てるようになった。
  
- PMとしてこれだけはやらなければならないこと。

  → 方向性を指し示す。ロードマップの根拠を提示する。顧客との会話。
  
- 新規プロジェクトでPdMがやるべきこと。

  → 顧客に話を聞く。とにかく調べて仮説をたてる。
  
----
#### 【リンケージxセレス】～失敗する開発とその対策～
  
https://connpass.com/event/281786/?utm_campaign=event_reminder&utm_source=notifications&utm_medium=email&utm_content=detail_btn
  
https://youtube.com/live/fSEH3kxcBQs?feature=share

「印象に残った点、気になった点」
- SaaS：失敗を避ける個社カスタマイズの原則。BtoB SaaSで個社カスタマイズは「やるな」。どうしてもやらなければいけない時はある
  - カスタマイズを一般的な機能のバリエーションまで昇華する
    - 要望を抽象化する
    - ドメイン知識を学ぶ
    - 複数企業にヒアリングする
  - カスタマイズを外しやすくする
    - カスタマイズ開発しても解約されることはあるので
    - interfaceを使ってデフォルト機能とカスタム機能を具象のバリエーションとして扱い切り替える。
    - コンパイルタイムで切り替えたい時はDI
    - マルチテナント： ランタイムで切り替えたい時はストラテジーパターン
    - 業種（地域）HogeServiceなどの名称にしておく

- 新人がリリース手順を間違えて２次障害まで発生してしまった。

  → 自分なりの考え
  - 再発防止策を講じるのであればリリース用のバッチファイルなどを作成した方が良い。なぜなら、この手のヒューマンエラーに対する再発防止策できちんと確認を怠らないようにするというのは曖昧であり個人に依るところが大きいため。

- コンテキスト図は必須

- 難易度の高いデータモデリングの場合、完璧なinterface抽象を１発で行うことはできない。
  - 検証無くして抽象は語れない
  - 具象と抽象を行き来する
  - 検証方法：リスコフ置換原則
  - クライアントの発言から本質を見出す。（雑談が重要）
  
- リファクタリングにおいて、単にコード重複を許さない考えだと死ぬ。目的の違うもの同士を抽象クラスやinterfaceで抽象化すると死ぬ。目的が違うと抽象化に失敗し、コードが混乱したりリスコフ置換原則に違反したりで死ぬ。
- アプリケーションはドメイン特化。再利用性を追いかけ無理に共通化すると容易にDRY原則に違反する。FWなど汎用レイヤーでは再利用性を追いかけて良い。
- 良い構造はたった一度の設計では見出せない。

----

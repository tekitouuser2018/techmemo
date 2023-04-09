### 特徴、性質
- 推論が得意なため、平均的な回答を行う。
- 分からない内容については適当な回答を行うため、内容を精査する必要がある。
- IT技術や法律などに詳しい。ソースコードやエラー内容をそのまま投げても解説が得られる。
- ベクトルを指定しながら質問や指示を行う必要がある。
- コードを書いてもらうときなどは、要件確認仕様確認など順序立てて指示を出すのが良い。
- アウトプットは使用者の知識や技量に左右される。
- ある程度文脈を記憶できる。


### 不得意
- 特殊ケース、エッジケース
- 曖昧さ
- 計算

### 注意点
- API以外のコンテンツでは、データが学習内容に利用されるため注意が必要。https://openai.com/policies/terms-of-use#:~:text=(c)-,Use,-of%20Content%20to
- 新しいシステムであるため様々な脆弱性があることが考えられる。
- 様々なサービスに組み込まれているが、それぞれ学習データの利用についてなど利用規約を確認する必要がある。
- 各企業内で使用上のルールを定める必要がある。（使用することのメリットが大きすぎるため、完全に使用不可にはしない方が良い）

### TIPS
- 回答が途中で途切れた時（特にコードの記述など）は、「途切れたところから続きを書いてください」と入力すると途切れた文章の先頭などから続きを書いてくれる。※「continue」「続けて」「続きを書いてください」などと入力すると、変なところから続きを書いてしまい、完全に前の文章と繋がらなかったりする。
- 話題のstep by stepで考えてもらうプロンプトは、「大項目」「中項目」「小項目」に分類して考えることと同じ？
<img width="1180" alt="スクリーンショット 2023-04-09 0 09 18" src="https://user-images.githubusercontent.com/38163743/230729002-975cff5c-ff55-46dc-a346-923d9fbfe5de.png">
- ChatGPTのパフォーマンスを最大限に引き出すためのプロンプト自体をChatGPTに質問する。
<img width="1181" alt="スクリーンショット 2023-04-09 0 09 06" src="https://user-images.githubusercontent.com/38163743/230729014-7a374653-d298-4796-a7a5-f28d51564bf2.png">
<img width="1178" alt="スクリーンショット 2023-04-09 0 09 40" src="https://user-images.githubusercontent.com/38163743/230729015-889d29cb-73e1-4e31-8b97-997deaa8f1b7.png">
<img width="1182" alt="スクリーンショット 2023-04-09 0 09 57" src="https://user-images.githubusercontent.com/38163743/230729023-a85c847f-5829-4968-8bbb-231d0583119c.png">
<img width="1181" alt="スクリーンショット 2023-04-09 0 10 31" src="https://user-images.githubusercontent.com/38163743/230729027-008e7904-75bc-42fe-90f7-8c6e0f3159e6.png">


### API
- 従量課金であり料金も安いが、設定の見直しが必要だったり日本語だとトークンの使用量が多いなど注意点がある。

https://openai.com/blog/openai-api

https://platform.openai.com/docs/api-reference

https://auto-worker.com/blog/?p=7428

https://auto-worker.com/blog/?p=7459

### プラグイン(α版)
https://openai.com/blog/chatgpt-plugins

https://www.techno-edge.net/article/2023/03/24/1056.html

### VSCode拡張機能(非公式)

https://github.com/ai-genie/chatgpt-vscode
<br>※下記の拡張機能が非推奨になりgenieの方で開発が続けられている様子https://github.com/gencay/vscode-chatgpt

https://maasaablog.com/integrated-development-environment/visual-studio-code/chat-gpt/5960/


### ツールの作成例など

- ChatGPT Embeddings APIとFaissを組み合わせて企業専用の社内資料などの知識をもったチャットボットの構築<br>
※文章を数百字のチャンクに分割したりしなければならないため、知識や工夫が必要

https://qiita.com/sakasegawa/items/16714fa132e874cab069

https://note.com/masa_kazama/n/n246df4af19f6

https://book.st-hakky.com/docs/faiss-overview/

https://platform.openai.com/docs/guides/embeddings/what-are-embeddings

### ダジャレや韻を踏んだ表現について

- 日本語の漢字やひらがなでそのまま韻を踏むよう指示を出すと何度か試しても思うような結果は得られない。また、「駄洒落」「ダジャレ」と指定すると存在しない言葉を出力することが多いため、ダジャレという言葉の意味をそのまま捉えて推論で回答している様子。（これは、辞書に存在する言葉に限定して出力するよう指定しても変わらなかった。）また、辞書内の単語はローマ字では持ち合わせていないためうまく韻を踏めないように感じる。
- ジョークやギャグなどは著作権に関わる可能性があるため、既存のものを学習させてはいない様子（少なくとも日本語では）
- 英単語の方が精度は高い。英単語で試すと、類似度判定でアルゴリズムを指定すると出力結果の趣が異なる。
- 日本語の場合、ローマ字表記に変換した上で綴りの類似度についてアルゴリズムを指定すると類似判定などが可能だが、精度はそんなに高くない。
<img width="1182" alt="スクリーンショット 2023-04-05 13 45 35" src="https://user-images.githubusercontent.com/38163743/230027464-fecb00ca-ff90-45c9-b069-d9c930c555cf.png">

<img width="1181" alt="スクリーンショット 2023-04-05 17 33 20" src="https://user-images.githubusercontent.com/38163743/230027753-5e9f0656-01df-4e04-bd66-a243f8be2577.png">

<img width="1179" alt="スクリーンショット 2023-04-05 17 33 41" src="https://user-images.githubusercontent.com/38163743/230027796-db76a7c2-1229-4dae-ab87-7962410f8c5d.png">

<img width="1180" alt="スクリーンショット 2023-04-05 17 34 24" src="https://user-images.githubusercontent.com/38163743/230027827-3a4457a2-e7cf-4384-b570-c4bde78054fb.png">

<img width="1180" alt="スクリーンショット 2023-04-05 17 43 46" src="https://user-images.githubusercontent.com/38163743/230029613-84e842da-7a61-4356-82cd-da41f8e4d78d.png">

<img width="1180" alt="スクリーンショット 2023-04-05 17 44 01" src="https://user-images.githubusercontent.com/38163743/230029653-6673e911-96eb-4d5c-8b0c-c1dff560c895.png">

<img width="1180" alt="スクリーンショット 2023-04-05 17 51 07" src="https://user-images.githubusercontent.com/38163743/230031364-addda859-c43d-48c2-8249-b738818afe66.png">

- 特定のアルゴリズムなどを用いてGPTが直接計算することはできないため、数値などは推論などでシミュレーションの結果を表示しているだけの様子️（やりとり内でシミュレーションの結果を表示しているという記述を見かけた記憶がある）
- pythonなどで実際にコードを書いてもらうことにより計算結果を出力してもらえる。

<img width="1181" alt="スクリーンショット 2023-04-07 15 28 55" src="https://user-images.githubusercontent.com/38163743/230554560-1117687c-e904-4879-ab5d-633ec0a7d96e.png">
<img width="1180" alt="スクリーンショット 2023-04-07 15 29 55" src="https://user-images.githubusercontent.com/38163743/230554620-da747a24-8f69-47a5-9d63-4a2442559168.png">
<img width="1180" alt="スクリーンショット 2023-04-07 15 30 06" src="https://user-images.githubusercontent.com/38163743/230554642-ecf57262-9352-4694-8815-ee0e3497703c.png">

----

### アイデア

- クイズゲームBot
<img width="1181" alt="スクリーンショット 2023-04-09 15 42 58" src="https://user-images.githubusercontent.com/38163743/230758735-522bdc88-596e-4125-bd4d-f7e06b72bc11.png">

- シミュレーションによる訓練
<img width="1180" alt="スクリーンショット 2023-04-09 15 44 06" src="https://user-images.githubusercontent.com/38163743/230758743-d6963623-c86c-4ea1-9ff0-ad8f95d6e145.png">

----

### その他

<img width="1180" alt="スクリーンショット 2023-04-09 15 43 36" src="https://user-images.githubusercontent.com/38163743/230758848-30077f3a-b2d0-4192-9eec-32b7736fc583.png">

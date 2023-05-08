### WebRTC

> WebRTC（Web Real Time Communications）とは、Webブラウザを利用してビデオ・音声・データなどを端末同士でリアルタイムにやり取りできる通信規格のこと。WebRTCは2011年頃にGoogleによってオープンソース化されています。主要なブラウザが対応しているため、ビデオ会議システムのような特別な機器を必要としないのが特徴です。開発・導入へのハードルが下がれば、多くの企業・店舗がビデオ通話アプリに注目するのは当然でしょう。

> WebRTCの基本的な仕組みは、利用する端末同士で双方向コミュニケーションする「P2P（Peer to Peer）通信」であること。SkypeやLINE通話などに採用されている通信方式ですが、データのやり取りにサーバを介さないことがポイントです。


### Twilio

> 「Twilio（トゥイリオ）」は、サンフランシスコに本社を構えるTwilio社が開発・提供するクラウドAPIサービスです。全世界の16万社以上から利用されており、日本ではKDDIウェブコミュニケーションズが窓口になっているため、安心して利用できるのもポイントです。
WebRTCを基盤にした「Programmable Video」サービスは、用途に合わせた4つのプランを用意。最大50人までのビデオ会議に対応するほか、音声録音、ビデオ録画にも対応可能。JavaScript / iOS / AndroidのSDKが用意され、SMSやチャットに対応するAPIサービスも利用できます。
米国企業が展開しているサービスですが、国内ではKDDIが代理店として販売しています。サービスの歴史も長く、WebRTC全般への知見も深いのが特長です。Community Editionは接続回数や転送量の上限がありますが、無料のためすぐに利用開始が可能。どんなものかを知りたい場合には便利です。

株式会社KDDIウェブコミュニケーションズによる Twilioサービスの提供終了のお知らせ
> 株式会社KDDIウェブコミュニケーションズは、Twilio Inc.（米国） の意向による販売代理店契約の終了に伴い、2023年5月1日（月）をもちまして、弊社によるTwilioサービスの提供を終了することになりました。
https://www.kddi-webcommunications.co.jp/news/info/20230317.html

初期費用：無料（トライアルあり）

月額費用：ポイントチャージの従量課金制

機能：グループ会議、音声録音、
ビデオ録画（SMS、チャットなどのAPIも利用可能）

SDK（ソフトウェア開発キット）：JavaScript / iOS / Android

https://www.twilio.com/ja/video

### agora

> 「agora」は、シリコンバレーで起業された「agora.io」が開発・提供するクラウドAPIサービスです。WebRTCと互換性のある独自技術「SD-RTN（仮想広域ネットワーク）」による、滑らかな動画リアルタイム配信を可能にしていることが特徴。日本ではビデオ会議で知られるブイキューブ社が総代理店を務めています。
音声通話、ビデオ通話、ライブ配信、リアルタイムメッセージなどのAPIが用意され、JavaScript / iOS / Android / Windows / macOS / React / Unityなど、フレームワークやIDEにSDKを組み込むことも可能です。リアルタイムでの動画配信に向いています。
米国企業が提供するサービスですが、日本国内では株式会社ブイキューブが総代理店として販売だけでなく技術サポートまで行っています。WebRTCの最大の課題であった大規模なアクセスに耐えられないという課題を解決し、同時に100万人規模という大規模な通信を可能にしました。コラボ配信についても、10,000人まで対応可能です。4Kまでの高画質にも対応。利用分数に応じた従量課金で利用できるため、必要な分だけしかかからないというのもメリットです。



初期費用：無料（トライアルあり）

月額費用：従量課金制（開発期間は無料）

機能：音声通信、ビデオ通話、ライブ配信、リアルタイムメッセージなど

SDK（ソフトウェア開発キット）：JavaScript / iOS / Android / Windows / macOS / React / Unity

https://jp.vcube.com/sdk


https://system-kanji.com/posts/call-application-development

https://jp.vcube.com/sdk/blog/summary-of-webrtc-commercial-services.-twilio-skyway-sora-agora.io-etc.html

----

### HOW TO

#### Twilio

Twilioの機能だけでサーバーレスなビデオ通話アプリが作れる

https://dev.classmethod.jp/articles/making-a-video-conferencing-app-with-twilio-webrtc-hands-on/

https://qiita.com/mobilebiz/items/bdace9699ce77dbc7279

Twilio Functionsは、本番レベルのイベント駆動型Twilioアプリケーションを、短時間で作成することを支援するサーバーレス環境です。ビジネスの規模に応じて拡張できます。

https://www.twilio.com/ja/docs/runtime/functions


----




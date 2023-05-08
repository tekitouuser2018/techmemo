### JWTとは
> JWTはHTTPヘッダーで利用することを前提としたJSONの表現方法を定めているだけで、セキュリティについてはなにも考慮していません。データは単にBase64URLエンコードしているだけで、Base64URLデコードすることで簡単に内容を見ること(盗聴)ができます。また、デコードしたデータを修正してエンコードし直すことで簡単にデータを改ざんすることもできます。このため、JWT単独で重要なデータを扱うには問題があります。
このJWTの改ざんを防止する手段としてJSON Web Signature(JWS)[1]があります。ただし、改ざん防止といってもデータを改ざんできないようにするのではなく、JWSはデータが改ざんされたらそれを検知する仕組み、もっというと受け取ったJWTが本物かどうかを確認する仕組みとなります。

> JWSの署名<br/>
改ざん防止で登場するのが暗号鍵を使ったデータの署名です。
暗号鍵を使ったJWTの署名は次の図をもとに説明します。なお、説明は理解を容易にするため、暗号化する側と復号化する側の双方が同じ鍵を使う共通鍵方式(秘密鍵方式)を前提にしています。公開鍵方式の説明は共通鍵で全体の仕組みを解説した後に簡単に行います。

> - クレーム(claim)<br/>
JSONのデータ項目をJWTのコンテキストで説明するときの用語。実体としてはJSONのデータ項目と同じ
複数のクレーム集合をクレームセットといい、これがJSONデータの全体に相当する
> - JWT(JSON Web Token)<br/>
JWSによりエンコードされたクレームセット<br/>
言い方を変えるとJWSのペイロードになるものがJWTともいえる<br/>
> - JWSコンパクトシリアライゼーション<br/>
ヘッダーとペイロード(JWT)とシグニチャをBASE64URLエンコードして.(ドット)で連結すること<br/>
> - JWS(JSON Web Signature)<br/>
JWTをJWSコンパクトシリアライゼーションしたもの<br/>
つまりJWSとはJWSコンパクトシリアライゼーションした次の文字列を指すBASE64URLエンコード(ヘッダー) + . + BASE64URLエンコード(ペイロード) + . + BASE64URLエンコード(シグニチャ)

> RFC的にJWSのデータ部に相当するものをJWTと呼ぶ定義となっているため、JWTとJWSは相互に依存した概念[4]となっています。このため、JWTやJWSの説明が難しくなっていますが、実際のところ、JWTはJSONの表現形式で、そのJWTをセキュアにやり取りできるようにJWSシリアライゼーションした文字列がJWSと理解して困ることはありません。
また、JWTはJWSの部分に対する呼び方のため、概念としてJWTが単独で存在することはありませんが、一般的にJWSのことをJWTとして呼んでいることも多くあります。ですので、次のJWTによる認証では一般的な呼び方にならいJWSはJWTとして説明します。

> JWT認証とは認証情報が設定されたJWTをもとにユーザ認証を行うことです。
もっと、平たくいうと、ユーザの認証方式として、自身のIDとそのユーザしか知り得ないパスワードを提示ししてもらうことでそのユーザが確かにIDに対する本人であることを識別するユーザログインが一般的ですが、JWT認証はIDとパスワードの代わりに自身が信頼していているアプリが発行した認証情報のJWTを提示してもらい、そのJWTを信頼したアプリと取り交わした鍵情報で検証することで、JWTに書かれているユーザ本人だと確認することとなります。

> JWEはJWT全体を暗号化して中身を見られなくする技術です。JWEを理解するにはその元データとなるJWTや関連するJWSへの理解が必要です。また、OpenIDConnectやOAuth2.0は主にアプリがIDプロバイダーからJWTのIDトークンを取得するまでの一連の流れを規定しているものとなります。よって、今回のベーシックなJWT認証の仕組みに対する理解なくしてOpenIDConnectやOAuth2.0の理解はおぼつきません。

https://developer.mamezou-tech.com/blogs/2022/12/08/jwt-auth/

https://www.logicmonitor.jp/blog/what-are-json-web-tokens

https://qiita.com/doyaaaaaken/items/02357c2ebca994160804

> WebアプリケーションでJWTをセッションに使う際の保存先は（自分なりに説明できれば）どちらでもよいと思います
> CookieだろうがlocalstorageだろうがどっちもXSSの前には無力です

> - CookieはJWTが漏れないだけで攻撃ができないわけではない<br/>
>   - リクエストを投げればブラウザが勝手につけるので、XSSで目的のリクエストを投げればいいので無力<br/>
> - localstorageはJWTも漏れるし、Cookieと同様にリクエストを投げるロジックを呼べばよい<br/>
>   - こっちはブラウザが勝手につけるわけではないが、XSSできるなら取得してリクエストにつけて投げるのも簡単なので無力<br/>
とはいえ、Cookie(httponly)ならJavaScriptからどうあがいても読み取ることはできません。 そういった意味ではlocalstorageよりもCookieのほうがセキュアかもしれません。ただしhttponlyなCookieだろうがXSSで攻撃に繋げられるのであまり意味はないこともあります。

> 「ランダムなセッションID」はセッションIDだけ見ても情報はわかりません。しかし、そこからサーバサイドに問い合わせてどういうユーザか等の情報を集めるということはできます。<br/>
一方、JWTはペイロードの内容から情報を抜き取ることができます。JWTをみれば誰がいつログインしたかとかの情報はわかってしまうかもしれません（持たせている情報によります） では暗号化しましょう、とJWE を採用するとか、独自にペイロード部分を暗号化するのもよいでしょうが、そこまでする価値はあるかというと、私は無いと思います。 ランダムなセッションIDを使った場合と同じで、結局サーバサイドに問い合わせてどういうユーザか等の情報を集めるということはできるからです。

> Chatworkさんの事例<br/>
> Chatworkさんの事例ではアクセストークンをステートレスで扱います。その代わり、期限を30分と短くしてその期限内の悪用は許容しています。 で、使い続けたい場合はリフレッシュトークンでアクセストークンを作り直してくれ、というやり方です。そこにログアウトはないということです。 その代わり再発行するためのリフレッシュトークンは結局ユーザに紐づけて管理しています。まぁ、ここを管理しないといくらでも再発行できちゃうor都度アクセストークンを発行させるような作業をユーザに強いることになりますからね。<br/>
これは頭が良くて、よく「セッション情報が流出したから強制ログアウトさせたい」みたいな運用を求めることがありますが、この運用自体が手遅れなんです。流出した事実はかわらないですからね。 パスワードのハッシュが流出したんでパスワードを再設定してください、というのも手遅れ。GitHubにAWSのアクセスキー流しちゃったとかも手遅れ。流出した事実はかわらないですからね。<br/>
そこで、流出したものが無意味になればどうでしょうか。パスワードのハッシュならハッシュに使うsalt値を変えれば概ね無意味になりますし、AWSのアクセスキーとかは失効できます。


https://ryozi.hatenadiary.jp/entry/2022/02/12/073014

https://twitter.com/ockeghem/status/1492786731607019523

JWTハンドブック<br/>
https://assets.ctfassets.net/2ntc334xpx65/5HColfm15cUhMmDQnupNzd/30d5913d94e79462043f6d8e3f557351/jwt-handbook-jp.pdf

----

### RFC7519

> JSON Web Token（JWT）は、2つのパーティ間で転送されるクレームを表す、コンパクトでURLセーフな手段です。 JWTのクレームは、JSON Web Signature（JWS）構造のペイロードとして、またはJSON Web Encryption（JWE）構造のプレーンテキストとして使用されるJSONオブジェクトとしてエンコードされ、クレームをデジタル署名または整合性保護することができます。メッセージ認証コード（MAC）で暗号化されています。

https://tex2e.github.io/rfc-translater/html/rfc7519.html

----

### RFC7516

JSON Web Encryption (JWE)は、JSONデータを暗号化するための標準規格であり、RFC 7516で定義されています。JWEは、JSON Web Token (JWT)のようなデータ構造を保護するために使用され、送信者と受信者間で機密データを安全にやり取りする際に役立ちます。JWEは、データを暗号化するために、JSON Web Algorithms (JWA)で定義された暗号アルゴリズムと鍵管理方式を使用します。

JWEの主な要素は以下のとおりです。

1. JWEの構造: JWEは以下の5つの部分で構成されます。

- JWE Header: トークンの暗号化アルゴリズムと鍵管理方式を含むJSONオブジェクトです。Base64Urlエンコードされます。
- Encrypted Key: コンテンツ暗号鍵（CEK）を暗号化したものです。CEKはデータを暗号化するために使用されます。
- Initialization Vector (IV): 暗号化アルゴリズムで使用される初期化ベクトルです。ランダム性を提供し、同じデータを暗号化した場合でも異なる結果が得られるようにします。
- Ciphertext: 暗号化されたデータ本体です。
- Authentication Tag: データの完全性と真正性を確認するためのタグです。
2. 暗号アルゴリズム: JWEでは、データを暗号化するためにさまざまな暗号アルゴリズムがサポートされています。これには、AES-GCM（ガロア/カウンターモードでのAES）、AES-CBC-HMAC（AESをCBCモードで使用し、HMACで認証する）などが含まれます。

3. 鍵管理方式: JWEは、コンテンツ暗号鍵（CEK）を管理するために、さまざまな鍵管理方式をサポートしています。これには、直接鍵管理（CEKを直接共有）、RSA鍵暗号化（RSA暗号化を使用してCEKを暗号化）、AESキーラップ（AESキーラップアルゴリズムを使用してCEKを暗号化）などが含まれます。

JWEは、データを暗号化して送信者から受信者へ安全に伝送するために設計されており、送信者の真正性を保証するものではありません。真正性の保証には、署名を使用したJSON Web Signature (JWS)を使用することが推奨されます。JWEとJWSを組み合わせることで、データの機密性、完全性、および送信者の真正性を保証することができます。これを達成するために、まずデータをJWSで署名し、その後JWEで暗号化することが一般的です。このアプローチは「署名後に暗号化」または「Nested JWT」と呼ばれます。

JWEを実装する際には、適切な暗号アルゴリズムと鍵管理方式を選択し、セキュリティ要件に応じて適切な暗号化強度を確保することが重要です。また、キー管理プロセスを確立し、鍵の生成、配布、更新、および廃棄に関するポリシーを遵守することも重要です。適切なセキュリティ対策が実施されている場合、JWEはデータの機密性と完全性を確保し、権限のある受信者だけがアクセスできるようになります。

> JSON Web Encryption（JWE）は、JSONベースのデータ構造を使用して暗号化されたコンテンツを表します。この仕様で使用する暗号化アルゴリズムと識別子は、別のJSON Web Algorithms（JWA）仕様と、その仕様で定義されているIANAレジストリで説明されています。関連するデジタル署名とメッセージ認証コード（MAC）機能については、別のJSON Web Signature（JWS）仕様で説明されています。

https://tex2e.github.io/rfc-translater/html/rfc7516.html

- JWE Compact Serialization
JWE Compact Serialization は、 BASE64URL エンコードされた5つのコンポーネントを、.(ピリオド)で結合するシリアライゼーション方式です。Web セーフなため、URL や HTTP ヘッダ値のなかで使用できます。

- JWE生成の手順
手順は大きく ①CEK生成 と ②コンテンツ暗号化 の２ステップで構成されます。

1. CEK生成
alg ヘッダパラメータによって指定されるアルゴリズムは、それぞれ対応する 鍵管理モード を持ちますが、この鍵管理モードにより定義される方法で、コンテンツを暗号化する コンテンツ暗号化キー(CEK) が生成または用意されます。鍵管理モードとして、5種類のモードが定義されています。

2. コンテンツ暗号化
enc ヘッダパラメータにより指定されたアルゴリズム(下表)でコンテンツを暗号化します。

- A128CBC-HS256
  - AES_128_CBC_HMAC_SHA256 認証付き暗号化アルゴリズム.
- A192CBC-HS384
  - AES_192_CBC_HMAC_SHA384 認証付き暗号化アルゴリズム.
- A256CBC-HS512
  - AES_256_CBC_HMAC_SHA512 認証付き暗号化アルゴリズム.
- A128GCM
  - 128ビットキーを使った AES GCM
- A192GCM
  - 192ビットキーを使った AES GCM
- A256GCM
  - 256ビットキーを使った AES GCM

3. JWE トークンを構築する
以下の5コンポーネントをピリオド'.'で連結し、JWEトークンの完成です。

①JWE Protected HeaderをUTF-8エンコード+Base64Urlしたもの<br/>
② 1 で求めたJWE Encryption Key をBase64Urlしたもの<br/>
③ 2 でランダムに生成したJWE Initial Vector を Base64Urlしたもの<br/>
④ 2 で暗号化した出力であるJWE Ciphertext を Base64Urlしたもの<br/>
⑤ 2 で暗号化した出力であるJWE Authentication Tag をBase64Urlしたもの<br/>

https://qiita.com/ono_matope/items/938a98fb111a297b68b9

----
### RFC7515

> JSON Web Signature（JWS）は、JSONベースのデータ構造を使用してデジタル署名またはメッセージ認証コード（MAC）で保護されたコンテンツを表します。この仕様で使用する暗号化アルゴリズムと識別子は、別のJSON Web Algorithms（JWA）仕様とその仕様で定義されているIANAレジストリで説明されています。関連する暗号化機能については、別のJSON Web Encryption（JWE）仕様で説明されています。

https://tex2e.github.io/rfc-translater/html/rfc7515.html

> JWSの構成要素について
JWSの仕様はいくらでも難しいことができるのですが、ここではよく使われている JWS Compact Serialization の利用を前提としています。

> 1. Header
> - 鍵の識別子として、必ず kid を含む
>   - 命名ルール : (用途)_201804_001 のような感じ
>   - 用途について : 用途により鍵を分けるという基本的な考えがあるので、kid に用途も含める
> - alg : 仕様上必須なので含む
>   - hsXXX : 例えば API Gateway が発行/検証を両方やるケースで利用する
>   - rsXXX, exXXX : 発行/検証が別のところで行われるケースで利用する
> 2. Payload
RFC7519の JWT Claims で定義されている iss, sub, aud, iat, exp のような値が使えそうなものは使う。iss, aud あたりの検証で脆弱性が生まれることを耳にするが、ここでは細かく決めない。
それ以外のパラメータ定義や、そもそもJSONにすらなってない形式を使いたければ自由。

> 3. Signature
仕様に従う。

https://qiita.com/ritou/items/a861e952ccea8ba9e68c

----
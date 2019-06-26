# TechMemo

### gin

----

- initAPI : 
```go:main.go
func init(){

	g := gin.New()
	
	if  !appengine.IsDevAppServer() {
		gin.SetMode(gin.ReleaseMode)
		// middlewareが走ってしまうので、初期登録時はここをコメントアウトする
		g.Use(middleware.UserAuth(util.APIPath(),util.AdminPath(),util.LoginPath(), util.TaskPath()))
	
	}
	
	g.NoRoute(func(c *gin.Context){
		c.FIle("dist/index.html")
	})
	initAPI(g)
	initAdminAPI(g)
	initLoginAPI(g)
	
	swaggerGin  := g.Group("/glm/swagger")
	swaggerGin.GET("/*any",ginSwagger.WrapHandler(swaggerFiles.Handler)	)
	http.Handle("/", g)
	
}

func initAPI(g *gin.Engine){
	apiGin := g.Group(util.APIPath())
	api.InitLoadingDataInformationAPI(apiGin)
}

```

----
### memo

- DB＝Excelデータ→スプレッドシート→KVS<br>
※データ取り扱いの処理をいちいち作らなければならない

<strong>・DBや外部ストレージなどに依存する機能の単体テストに関しては「モッキング」と「ローカル環境上に構築したダミーの外部サービスを使用する」方式の２種類がある。<font color=red>Dockerにより後者の方式のメリットが大きくなっている。(AWSの場合、各種サービスをエミュレート可能な`localstack`というDockerイメージが存在しますし、CircleCI等のCIサービスもDockerに対応しているので、ローカル上でもCI環境上でも同じ方式で単体テストが行える状況になっています)</font></strong>

<strong>インフラのコード管理</strong>
<font color=blue>[DeploymentManager]</font>

<font color=red>[Linter] JS - ESLint・Prettier</font>

シェルスクリプトでは_処理対象のデータはパイプラインやリダイレクトで標準入力から受け取って、処理結果は標準出力で出力するのがベストプラクティス_です。

	$ cat ecces.log | grep "error" > errors.log
	$ cat errors.log | cut -d " " -f 5 | sort | uniq -c > count.txt


<strong><font color=green>
- GraphQLのメリットを一言で言えば「クライアント＝サーバー間での複雑なトランザクション処理の全てをGraphQLが吸収してくれる」
- エンドポイントは一つ
- RESTの代替
- GraphQLの作者[Lee Byron](https://twitter.com/leeb)氏も言うように「なんでもできるGraphQLだが、GraphQLはできるだけ薄く保つのがいい」。GraphQLの責務と役割はquery（RESTfulのGET）とmutation（POSTもしくはPATCH）だけであって、それ以外は無い。</font></strong>

<strong>WebSocket</strong>
- WebSocketはクライアントとサーバー間でTCPコネクションを張りっぱなしにして通信を行うため、Blocking I/OをもつWebサーバとは相性が悪い。
- Node.jsが最近ではよく使われている。
- nginxもWebSocketに対応した。


----
### Docker

・ホストファイルのマウント：

	volumes:
		- ./myproxy/conf/proxy.conf:/etc/nginx/conf.d/proxy.conf

<strong><font color=blue>・Dockerコンテナのホストアドレスへの接続は特殊</font></strong>

	docker.for.mac.localhost:3306
	
	location / {
		proxy_pass http://docker.for.mac.localhost:8080;
	}

<strong><font color=red>・MySQLの初期データ投入</font></strong>
/docker-entrypoint-initdb.dに.sqlや.shファイルとして配置すると、コンテナ起動前に実行される。

	volumes: 
		- ./mysql/db:/docker-entrypoint-initdb.d




-----
### Angular Module import

```
	npm install --save @angular/material @angular/cdk
	npm install --save @angular/animations
```


### Angular version up

	npm install -g typescript@latest
	npm i typescript@3.4.5
	ng update --all --force
	


----- 

### Front

「サーバーサイドレンダリング（SSR）」
- 画面表示の高速化
- CPU負荷が高くなる→キャッシュやATFレンダリングなど対策が必要
- SEO対策に有効-GooglebotがJavascriptを実行できるため
- 変更が多いページ、動的なページには使えない
	
<strong><font color=red>
Time to Interactiveまでの時間も重要
- Code SplittingしてJS自体を軽くする
- HTTP/2のPUSHを使って次のリソースを事前キャッシュしておく
- DBクエリチューニング+ネットワーク改善
- CSS調整
- 画像の遅延読み込み
- JSの遅延ロード
- HTTP/2やResource Hintsによるロード改善

</font></strong>

----- 
<strong>Cloud Function = AWS Lambda</strong>
- Javascriptの単純な関数をクラウド上で実行する
- イベントベースで発火。Storage or Pub/SubまたはHTTPリクエスト
- ランタイムとしてNode.jsを使用。自動的にスケール

-----
<strong>Vue</strong><br>
- cli - プロジェクト作成
- Webpack - パッケージ管理？
- vuex - State管理

- コンポーネント毎に分けて管理
- components配下に部品を分けて格納

-----
<strong>Node.js</strong><br>
https://qiita.com/ymasaoka/items/3db3f44990911a181ffc<br>
- <strong>node.jsは、ブラウザの外部でJavaScriptを実行する、オープンソースかつクロスプラットフォームのJavaScriptランタイム環境。</strong>
<strong>フレームワーク</strong>
- Express.js
	- シンプルで早い
	- 学習コスト低い
	- フルカスタマイズ可能
- Meteor.js
	- より大きなプロジェクトを管理するための能力がある
	- 豊富で整理されたドキュメントコミュニティ
	- Facebook GraphQLデータスタックを利用している
	- 多くの開発者にとって理解が簡単である

-----
<strong>Webpack</strong><br>
- <strong>WebAppに必要なリソースの依存関係を解決し、アセットを生成するビルドツール。JS,CoffeeScript,TypeScript,CSS,画像ファイルなどを扱うことができる。</strong>

----

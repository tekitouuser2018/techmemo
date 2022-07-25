**Error Boundary**
> error boundary は以下のエラーをキャッチしません：<br/>
・イベントハンドラ（詳細）<br/>
・非同期コード（例：setTimeout や requestAnimationFrame のコールバック）<br/>
・サーバサイドレンダリング<br/>
・（子コンポーネントではなく）error boundary 自身がスローしたエラー<br/>
https://ja.reactjs.org/docs/error-boundaries.html

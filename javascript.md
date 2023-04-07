
### document.writeは, DOM > style > Layoutの流れがやり直しになるため、使うべきではない

https://speakerdeck.com/recruitengineers/browser-b45d3a59-af2b-449c-992e-fd7563745f80?slide=46

----

### オブジェクトのディープコピーにはstructuredClone関数を使う。

```txt
structuredClone関数はディープコピーを行うために作られた専用関数です。これを使えば従来のJSON化を使った方法のデメリットが解消できます。

JSON化を使った方法と比べた利点は以下です。

より多くの型がクローン可能
クローンできる型の一覧
処理時間的にも使用メモリ量的にも効率的
再起的な構造があっても動作する
structuredClone関数は主要なブラウザ全てで利用可能です。

```

https://qiita.com/silane1001/items/ae2e491abd64c01c7f3e

----

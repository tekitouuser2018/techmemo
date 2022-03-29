
#### ランダムでテストが失敗する仕様

https://github.com/ruby/ruby/blob/master/coverage/README#L12

#### rubyのカバレッジは行ベースであるため、三項演算子などワンラインで分岐が記述されていた場合には分岐を通らなくてもカバレッジの条件を満たしてしまう。

https://github.com/ruby/ruby/blob/2d5ecd60a5827d95449b9bd8704a0df2ffb0a60a/ext/coverage/coverage.c#L448..L476

これをカバーするためには、コーディングルールを設けたりレビューなどで各テストが十分か確認する必要がある

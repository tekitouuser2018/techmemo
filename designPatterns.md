### DI

- 何が問題なの?

> クラス内などで固定化されたものがあると<br/>
> 柔軟性がない<br/>
> テストしにくい

- 解決方法
> 「依存している部分を外から注入する」

- つまり、何が何に依存している?
> とあるクラスが、固定した他の(定数、変数、クラスなど)に依存している

- 依存していると、何が嫌なの?
> 外から動的に動作を変更できないので、テストしづらい<br/>
決め打ちなので、柔軟性がなくカスタマイズしにくい

- 具体的にどう困るの?
> あるクラスだけテストしたいのに<br/>
>  - 中に別のクラスが入っているとテストしにくい<br/>
>  - テストに時間がかかるメソッドが中にあってテスト終了に時間がかかる

- で、結局どうしろっての?<br/>
> 引数で、クラスや変数を外から受け取れるようにする

https://qiita.com/hshimo/items/1136087e1c6e5c5b0d9f

DIを使用するとMockを使用してテストしやすくなる

例：
```C#
public class Aggregator
{
    // DataGenerator の実装を new するのは外部にお任せ
    public Aggregator(IDataGenerator dataGenerator)
    {
        DataGenerator = dataGenerator;
    }

    private IDataGenerator DataGenerator { get; }

    public int Sum() => DataGenerator.Generate().Sum();
}
```

```C#
public class UnitTest1
{
    // テスト用の DataGenerator
    private class MockDataGenerator : IDataGenerator
    {
        public int[] Generate() => new[] { 1, 2, 3 };
    }

    [Fact]
    public void Test1()
    {
        // テスト時はテスト用の DataGenerator を使う
        var aggregator = new Aggregator(new MockDataGenerator());
        // テスト用の DataGenerator は固定値を返すのでテスト出来る
        Assert.Equal(6, aggregator.Sum());
    }
}
```
> こんな感じで、テスト時に差し替え可能に出来たほうが嬉しいものに対して interface を定義して実装を外部から設定するようにするといい感じですが、クラスの利用者側視点から見ると割と最悪です。例えば Aggregator を使う場合は Aggregator の他に DataGenerator を new しないといけないです。<br/>
もっと複雑なものになると記述が大変なことになる。

> ということで、オブジェクトの依存関係を外部から設定するようにすると、オブジェクトを組み立てるのが大変になるので、そこを省力化しようというライブラリが登場してきます。<br/>
それが DI コンテナです。

Spring boot, LaravelなどではDIコンテナの機能がある。

https://qiita.com/okazuki/items/a0f2fb0a63ca88340ff6

> Rubyでは、動的にクラスに生えているメソッドを書き換えられるため、テスタビリティを上げるためにDIを使わない。

> - 言語ごとに原理原則は変わるし、何を犠牲にしているかも違う
> - 誤った依存関係だろうが何だろうが、メソッド内に持っていいということではない<br/>
> - 凝集度と結合度は変わらず意識すべき<br/>
> - Abstract FactoryパターンやStrategyパターンなど、RubyでもDIが実装上有用なケースは普通にある

https://blog.hiko1129.com/2019/09/rubydi.html

https://zenn.dev/hihats/articles/dependency_injection_is_not_a_virtue

----

### オブジェクト指向

Javaの場合:
```java
public interface CreditCard {
    boolean isValid();
}

public class VisaCard implements CreditCard {
    private String number;

    public VisaCard(String number) {
        this.number = number;
    }

    @Override
    public boolean isValid() {
        // チェックアルゴリズム
        return number.startsWith("4");
    }
}

public class MasterCard implements CreditCard {
    private String number;

    public MasterCard(String number) {
        this.number = number;
    }

    @Override
    public boolean isValid() {
        // チェックアルゴリズム
        return number.startsWith("5");
    }
}


```

Goの場合:

```go
type CreditCard interface {
    IsValid() bool
}

type VisaCard struct {
    number string
}

func (v VisaCard) IsValid() bool {
    // チェックアルゴリズム
    return strings.HasPrefix(v.number, "4")
}

type MasterCard struct {
    number string
}

func (m MasterCard) IsValid() bool {
    // チェックアルゴリズム
    return strings.HasPrefix(m.number, "5")
}

```


階層が深い場合

Java
```java
public class Animal {
    public void eat() {
        System.out.println("Eating...");
    }
}

public class Bird extends Animal {
    public void fly() {
        System.out.println("Flying...");
    }
}

public class Sparrow extends Bird {
    public void chirp() {
        System.out.println("Chirping...");
    }
}


```

Go : コンポジションを使用する
```go
type Eater interface {
    Eat()
}

type Animal struct{}

func (a Animal) Eat() {
    fmt.Println("Eating...")
}

type Flyer interface {
    Fly()
}

type Bird struct {
    Animal // コンポジションを利用
}

func (b Bird) Fly() {
    fmt.Println("Flying...")
}

type Chirper interface {
    Chirp()
}

type Sparrow struct {
    Bird // コンポジションを利用
}

func (s Sparrow) Chirp() {
    fmt.Println("Chirping...")
}

```

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

****

### DI

JavaのSpring Bootでは依存性の注入（DI）はフレームワークによって管理されており、クラスが他のクラスに依存している場合、その依存性は自動的に注入されます。これは主に@Autowiredアノテーションを使用して行われます。

Java
```java
@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // その他のメソッド
}


```

Go

GoにはSpring Bootのようなフレームワークが存在せず、自動的な依存性の注入機能はありません。そのため、依存性は手動で注入する必要があります。通常、これはコンストラクタを通じて行われます。

```go
type UserRepository interface {
    // その他のメソッド
}

type UserService struct {
    userRepository UserRepository
}

func NewUserService(userRepository UserRepository) *UserService {
    return &UserService{userRepository: userRepository}
}

// その他のメソッド


```


ここでは、UserServiceはUserRepositoryに依存しています。NewUserService関数は、UserRepositoryの依存性を注入するためのコンストラクタとして機能します。

もしこれにさらに高度な依存性の管理や自動的な注入が必要な場合、Goで使用できるDIフレームワークやライブラリ（例えば、uber-go/digやgoogle/wire）を利用することも可能です。ただし、Goの開発者の間では、可能な限り明示的な依存性の注入とシンプルな設計を推奨する傾向にあります。

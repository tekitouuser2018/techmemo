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

****

### ルーティング

Spring Boot:

Spring Bootでのルーティングは通常、Controllerとそのメソッドに注釈を付けることにより行います。以下に例を示します。

```Java
package main

import "github.com/gin-gonic/gin"

func main() {
    r := gin.Default()

    r.GET("/hello", func(c *gin.Context) {
        c.String(200, "Hello, World!")
    })

    r.GET("/goodbye", func(c *gin.Context) {
        c.String(200, "Goodbye, World!")
    })

    r.Run() // listen and serve on 0.0.0.0:8080
}
```

GoのGin:

GoのGinフレームワークでは、ルーティングはルートグループとハンドラ関数を使って定義します。以下に例を示します。

```go

package main

import "github.com/gin-gonic/gin"

func main() {
    r := gin.Default()

    r.GET("/hello", func(c *gin.Context) {
        c.String(200, "Hello, World!")
    })

    r.GET("/goodbye", func(c *gin.Context) {
        c.String(200, "Goodbye, World!")
    })

    r.Run() // listen and serve on 0.0.0.0:8080
}
```

上記の例では、"/hello"と"/goodbye"の2つのエンドポイントを定義しています。各エンドポイントは、そのハンドラ関数（ここでは匿名関数）を持っています。

***

### データアクセス

Spring Boot（JPA）の場合:

まず、エンティティクラスを定義します。

```Java

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
public class User {

    @Id
    @GeneratedValue
    private Long id;
    private String name;

    // getters and setters...
}
```

そして、リポジトリインターフェースを定義します。

```Java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    User findByName(String name);
}
```

これにより、Spring Data JPAは基本的なCRUD操作（保存、検索、更新、削除）を自動的に提供します。特定の名前でユーザーを検索するメソッドも定義しました。

Go（GORM）の場合:

まず、モデルを定義します。

```go
package main

import (
    "github.com/jinzhu/gorm"
    _ "github.com/jinzhu/gorm/dialects/sqlite"
)

type User struct {
    gorm.Model
    Name string
}
```

そして、データベースの操作を行います。

```go
package main

import (
    "github.com/jinzhu/gorm"
    _ "github.com/jinzhu/gorm/dialects/sqlite"
)

type User struct {
    gorm.Model
    Name string
}

func main() {
    db, err := gorm.Open("sqlite3", "test.db")
    if err != nil {
        panic("failed to connect database")
    }
    defer db.Close()

    // Migrate the schema
    db.AutoMigrate(&User{})

    // Create
    db.Create(&User{Name: "L123"})

    // Read
    var user User
    db.First(&user, "name = ?", "L123")
}
```

上記の例では、SQLiteデータベースに接続し、スキーマをマイグレートし、新しいユーザーを作成し、そしてそのユーザーを読み込んでいます。

注意点としては、GoとGORMでは、データベースとの接続、スキーマのマイグレーション、エンティティの保存と読み込みなど、すべてを手動でコントロールする必要がある点です。一方、Spring BootとJPAでは、これらの多くの操作が自動化されています。

また、各フレームワークにはそれぞれ様々な機能があるため、この例は一部を簡略化しています。具体的な移行プロセスでは、トランザクション管理、クエリの最適化、懸念の分離、テスト戦略など、さまざまな要素を考慮する必要があります。




より複雑なクエリの例として、User, Order, そして Product の三つのエンティティに対するクエリを考えてみましょう。ここでは、特定のユーザーが購入した商品の中で、特定のカテゴリーに属する商品の注文のみを取得し、注文日の新しい順に並べ、数量が一定以上のもののみをフィルタするという操作を考えます。

Spring Boot（JPA）:

まず、User、Order、Productのエンティティを定義します。

```Java
import javax.persistence.*;
import java.util.Set;

@Entity
public class User {
    @Id
    @GeneratedValue
    private Long id;
    private String name;

    @OneToMany(mappedBy = "user")
    private Set<Order> orders;

    // getters and setters...
}

@Entity
public class Order {
    @Id
    @GeneratedValue
    private Long id;
    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
    @ManyToOne
    @JoinColumn(name = "product_id")
    private Product product;
    private Integer quantity;

    // getters and setters...
}

@Entity
public class Product {
    @Id
    @GeneratedValue
    private Long id;
    private String name;
    private String category;

    // getters and setters...
}
```

そして、UserRepositoryを作成します。

```Java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.domain.Sort;

public interface UserRepository extends JpaRepository<User, Long> {
    @Query("select o from Order o join fetch o.user u join fetch o.product p where u.name = ?1 and p.category = ?2 and o.quantity >= ?3 order by o.createdAt desc")
    List<Order> findUserOrdersInCategoryWithQuantity(String userName, String category, Integer minQuantity);
}
```

ここでは、特定のユーザー名とカテゴリーを持ち、かつ注文量が一定以上の注文を取得するためのメソッドを作成しました。

Go（GORM）:

まず、User、Order、Productのモデルを定義します。

```go
package main

import (
    "github.com/jinzhu/gorm"
    _ "github.com/jinzhu/gorm/dialects/sqlite"
    "time"
)

type User struct {
    gorm.Model
    Name  string
    Orders []Order
}

type Order struct {
    gorm.Model
    UserID uint
    ProductID uint
    Quantity int
    Product Product `gorm:"foreignKey:ProductID"`
    User User `gorm:"foreignKey:UserID"`
}

type Product struct {
    gorm.Model
    Name string
    Category string
}
```

そして、特定のユーザーの特定のカテゴリーの注文を取得するクエリを作成します。

```go
package main

import (
    "github.com/jinzhu/gorm"
    _ "github.com/jinzhu/gorm/dialects/sqlite"
)

func main() {
    db, err := gorm.Open("sqlite3", "test.db")
    if err != nil {
        panic("failed to connect database")
    }
    defer db.Close()

    // Migrate the schema
    db.AutoMigrate(&User{}, &Order{}, &Product{})

    var orders []Order
    db.Joins("join users on users.id = orders.user_id").Joins("join products on products.id = orders.product_id").
        Where("users.name = ? AND products.category = ? AND orders.quantity >= ?", "John", "Electronics", 10).
        Order("orders.created_at desc").Find(&orders)
}
```

この例では、"John"という名前のユーザーが"Electronics"というカテゴリーの商品を購入した注文を取得し、注文日の新しい順にソートし、さらに注文量が10以上のもののみをフィルタリングしています。

****


# Static Factory Method

(정적 팩토리 메서드)

## Java의 생성자

 Java의 생성자(`constructor`)는 클래스(인스턴스)를 초기화 하는 가장 기본적인 방법이다. 인스턴스의  상태를 초기화 하거나 종속성을 주입하는데 사용된다. `생성자` 라는 이름에 맞게 객체를 생성하는 기능을 한다. 

#### `Book.class`

```java
class Book {
    String title;
    String author;
    String genre;

    public Book(String title, String author, String genre) {
        this.title = title;
        this.author = author;
        this.genre = genre;
    }
}
```

`Book` 이라는 인스턴스를 생성하기 위해선 다음과 같은 코드를 통해 수행할 수 있다.

```java
Book b1984 = new Book("1984", "GeorgeOrwell", "fiction");
Book animalFarm = new Book("Animal Farm", "GeorgeOrwell", "fiction");
...
```

`Book`에는 모든 필드를 주입받는 생성자가 명시되어 있어, 생성하기 위해 모든 상태를 기입하여야 한다. 좀 더 유연하게 생성하기 위해 다음과 같은 생성자들을 만들어 활용해 보자.

```java
class Book {
    String title;
    String author;
    String genre;
  
  	public Book(String title) {
        this.title = title;
    }
  
  	public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    public Book(String title, String author, String genre) {
        this.title = title;
        this.author = author;
        this.genre = genre;
    }
}

Book MobiDick = new Book("The Whale");
Book stoner = new Book("Stoner", "John Williams");
Book b1984 = new Book("1984", "GeorgeOrwell", "fiction");
Book animalFarm = new Book("Animal Farm", "GeorgeOrwell", "fiction");
```

주입하는 필드가 다르지만, 모두 `new Book()`이라는 이름의 생성자를 호출하여야 한다는 점을 확인 할 수 있다.

다 같이 `Book`을 생성하긴 하지만, 어떤 생성자는 제목만으로도 생성하고 있으며 어떤 생성자는 모든 상태를  주입받고 생성한다.

다 같은 인스턴스 생성이라는 역할을 하지만 구체적인 명세는 조금 다르다. 이름을 주어 분명하게 하고 싶다.



## Static Factory Method(정적 팩토리 메서드)

위 사례와 같은 사소한 불편함이, `Java`와 같은 객체 지향 언어의 생성자가 틀렸다고 할 수 있는가? 전혀 아니라고 생각한다.
하지만, Josua Block의 Effective Java에서는 분명하게 말하고 있다.

> ***“Consider static factory methods instead of constructors”***
>
> \- *객체를 생성하는 역할을 분리하고자 `Factory`라는 키워드가 붙은 것 같다.*

그리고, 다음과 같은 장점들이 위 주장의 설득력을 높이고 있다.

### 1. **생성자는 의미있는 이름을 가지지 못한다**. 항상 프로그래밍 언어에 의해 부과되는 표준 명명 규칙에 제한된다.

​	-> 정적 팩토리 메서드는 의미있는 이름을 가짐과 동시에 하는 일을 명시적으로 전달할 수 있다.

```java
class Book {
    String title;
    String author;
    String genre;
  
  	private Book() {}

    public static Book withTitle(String title) {
        Book book = new Book();
        book.title = title;
        return book;
    }

    public static Book withTitleAndAuthor(String title, String author) {
        Book book = new Book();
        book.title = title;
        book.author = author;
        return book;
    }
}

Book.withTitle("The Whale");
Book.withTitleAndAuthor("Stoner", "John Williams");
```

​	좋은 코드는 아니지만, 위처럼 이름을 부여하여 명시가 가능하다.

> #### `java.util.Optional`
>
> ```java
> Optional<String> value1 = Optional.empty();
> Optional<String> value2 = Optional.of("Baeldung");
> Optional<String> value3 = Optional.ofNullable(null);
> ```



### 2. 정적 팩토리 메서드는 **다양한 타입을 반환할 수 있는 유연함**을 갖추었다.

​	-> 특히, 하위 타입의 객체를 반환 할 수 있는 것이 큰 장점이 될 수 있다.

```java
public Collections() {
     //...

    public static final List EMPTY_LIST = new EmptyList<>();

    public static final <T> List<T> emptyList() {
        return (List<T>) EMPTY_LIST;
    } 
   //...
}

public static void main(String[] args) {
  System.out.println(Collections.emptyList().getClass().getName()); //Collections$EmptyList
  System.out.println(Collections.EMPTY_SET.getClass().getName()); //Collections$EmptySet
}
```

​	

### 3. 정적 팩토리 메서드는 싱글톤과 같이, **인스턴스를 제어 할 수 있는 메서드**가 될 수 있다.

​	-> 메서드 내에서 인스턴스를 직접 제어하며 관리가 가능하다

```java
public class User {
    
    private static volatile User instance = null;
    
    // other fields / standard constructors / getters
    
    public static User getSingletonInstance(String name, String email, String country) {
        if (instance == null) {
            synchronized (User.class) {
                if (instance == null) {
                    instance = new User(name, email, country);
                }
            }
        }
        return instance;
    }
}
```



### 4. 정적 팩토리 메서드는 **객체 생성을 캡슐화** 할 수 있다.

​	-> 객체의 모든 필드를 주입하지 않더라도, 메서드 내의 로직을 통해 객체를 생성할 수 있다.

```java

class BookDto {
    private String title;
    private String author;

    public BookDto(String title, String author) {
        this.title = title;
        this.author = author;
    }
  //getter..
}

class Book {
    private String title;
    private String author;
    private LocalDateTime createdAt;

    private Book(String title, String author, LocalDateTime createAt) {
        this.title = title;
        this.author = author;
        this.createdAt = createAt;
    }


    public static Book createByDto(BookDto bookDto) {
        LocalDateTime now = LocalDateTime.now();
        return new Book(bookDto.getTitle(), bookDto.getAuthor(), now);
    }

    //getter...
}

public static void main(String[] args) {
  BookDto bookDto = new BookDto("1984", "GeorgeOrwell");
  Book.createByDto(bookDto);
}
```



## 결론

정적 팩토리 메서드는 단순히 생성자를 대신하는 것이 아니다. 객체의 생성에 대한 명시적인 표현을 통해 가독성을 높힐 수 있고, 객체의 생성에 여러 기능이나 경중을 부과하여 그 의미를 달리할 수 있다.

생성자보다 정적 팩토리 메서드를 고려하라는 말은 은탄환이 아니라는 말과 함께 시작한다.
정적 팩토리 메서드만 존재하는 객체는 상속이 불가 하고, 컨벤션 상 쉽게 드러나 보이는 생성자에 비해 일반 메서드로 분류되어 객체 생성에 대한 명세를 확인하기 어렵게 할 수도 있다.

그럼에도 불구하고 항상 고려하라고 독려되는 이유는 적절히 사용했을 때 얻을 수 있는 장점이 많기 때문이다.
특히 객체간 형 변환이 필요하거나, 객체 생성의 의미가 큰 경우, 그리고 객체가 자주 생성되는 경우에 대해서 장점이 쉽게 드러남을 알 수 있다.

정적 팩토리 메서드는 객체 생성에 이름을 부과 할 수 있는 장점이 있다. 그리고, 정적 팩토리 메서드의 이름을 짓는 데에 있어 어느 정도 규칙이 있고, 다음과 같다.

## 정적 팩토리 메서드 네이밍 컨벤션

- `from` : 하나의 매개 변수를 받아서 객체를 생성
- `of` : 여러개의 매개 변수를 받아서 객체를 생성
- `getInstance` | `instance` : 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음.
- `newInstance` | `create` : 새로운 인스턴스를 생성
- `get[OtherType]` : 다른 타입의 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음.
- `new[OtherType]` : 다른 타입의 새로운 인스턴스를 생성.


# Java의 Runtime Data Area

프로그램을 구동하기 위해 필요한 메모리 공간은, 컴퓨터에서 운영체제가 데이터 및 명령어를 저장할 공간을 할당해 준다. 이 메모리는 한 컴퓨터 내에서 하나의 프로그램 사용하는 것이 아니기 때문에 사용 공간이 한정되어 있고, 이를 어떻게 관리하느냐에 따라서 프로그램의 성능이 좌우된다. 

자바로 작성된 코드는 `.java`의 형태로 저장되는데, 이를 Java 컴파일러가 byte코드로 변경하여 `.java`->`.class` 로 저장한다. 작성한 코드를 1차적으로 숨기고, 더 이상의 문법 검사를 수행하지 않아 약간의 시간 단축을 기대할 수 있지만, 매번 컴파일러가 변경해야 하기 때문에 역설적으로 느려질 수 있다.

byte코드로 변경된 이 `.class` 파일을 `Class Loader` 가 JVM의 `Runtime Data Area` 로 로딩 시켜 프로그램을 수행한다. 이 `Runtime Data Area`는 JVM이 프로그램을 수행하기 위해 OS로부터 할당받는 메모리이다.



즉, 효율적인 메모리의 사용이 프로그램의 성능에 영향을 미친다는 것이다. 

### `Runtime Data Area`의 구성

- `PC Register`
- `Native Method Stack`
- `JVM Stack`
- `Heap` 
- `Method Area`



### PC Register

- Thread마다 존재

- PC는 Program Counter이지만 일반적인 CPU의 PC Register와는 다르다

  >CPU에서의 PC (Program Counter) : 다음 인출(Fetch) 될 명령어의 주소를 가지고 있는 레지스터

- Thread가 어떤 부분을 어떤 명령어로 수행할지를 저장하는 공간

  => 현재 수행중인 JVM의 Instruction(명령어)의 주소를 저장하는 공간



### Native Method Stack

- Thread마다 존재

- Java가 아닌 다른 언어로 작성된 코드를 위한 공간

  > JNI(Java Native Interface)를 통해 호출되는 C/C++의 코드
  >
  > 앞서 살펴보았던 HashCode()와 같은 메서드
  >
  > ```java
  > public class Object {
  >   //...
  >   @HotSpotIntrinsicCandidate
  >   public native int hashCode();
  >   //...
  > }
  > ```



### JVM Stack

- Thread마다 존재
- 지역변수나 메서드의 매개변수, 임시적으로 사용되는 변수, 메서드의 정보가 저장되는 영역. (Primitive type의 실제 값, Reference type의 참조값)
- 메서드가 호출될 때 메모리에 할당되고 해당 메서드가 종료되면 메모리가 해제된다.
- 각각의 Thread는 각자의 Stack에만 접근할 수 있다.
  다른 Thread의 Stack에는 접근할 수 없다는 말이다.

ex) JVM Stack의 간략한 동작 과정

```java
public class StackClass {
  public static void main(String[] args) {
    int a = 5; // 1
    a = operateNumber(a); // 2
  }
  
  private static int operateNumber(int number) {
    int operateResult = number * 2; // 3
    return operateResult; // 4
  }
} // 5
```

>  간단한 과정 설명을 위해 `args` 배열은 우선 무시하고 생각하자

우선 main Thread가 생성되어 실행하였으니 새로운 Stack도 생성된다. 

코드 `1` : `a`에 5라는 값을 할당했다. `a` 는 `int` 타입이므로 값(5)이 Stack에 할당된다.

> #####  Stack [a = 5]

코드 `2` :  `operateNumber` 메서드를 호출하였다. 
메서드를 호출하면서 `a` 를 넘겨주고 해당 메서드로 scope가 이동한다. 넘겨받은 변수의 값을 메서드의 파라미터인 `number` 에 복사하고, 타입이 `int` 이므로 Stack에 값이 할당 된다.

> #####  Stack [~~a = 5~~, number = 5]
>
> 취소선은 scope외의 영역임을 뜻함

코드 `3`: `operateResult`가 선언되었다.

>#####  Stack [~~a = 5~~, number = 5, operateResult = 10]

코드 `4` : `return`을 통해 `a`에 `operateResult` 값을 반환해주고 메서드를 종료한다.
메서드가 종료되면서 현재 scope의 지역변수들이 모두 `pop` 된다.

>##### Stack [a = 10]

코드 `5` : main 메서드도 종료되면서 Stack의 모든 데이터들도 pop이 되고 프로그램이 종료된다

> ##### Stack []



- Stack은 크기가 정해져 있는 타입이다. 메서드 작업이 종료되면 해당 Scope에서 사용하였던 메모리가 알아서 비워지기 때문에 메모리 누수와 같은 문제에 대해서 신경쓰지 않아도 된다. 하지만 크기가 정해져 있다 보니 할당 받을 수 있는 최대 메모리에 제약이 있다는 특징이 있다.



### Heap

- Thread의 수와 관계 없이 하나만 존재하며, 모든 Thread가 공유한다.

- GC(Garbage Collector)의 대상이 되는 공간

- 동적으로 생성된 객체가 저장되는 영역이다.
  `new` 연산을 통해 생성된 인스턴스 변수가 저장됨
  ex) `new Person();`, `new int[10]`

- 참조변수는 Stack 영역에 참조값(해시코드)이 저장되고 new 연산자를 통해 Heap 영역의 해당 데이터를 리턴 받는다.

  `String url = "localhost:8080"`

  > ##### Stack = [url = @15db9742]
  >
  > ##### Heap = [String | localhost:8080]

  `url`에 저장된 참조값(@15db9742)은 Heap 영역의 `"localhost:8080"`라는 스트링 타입을 레퍼런스 하고 있다.

좀 더 복잡한 예제

```java
public class HeapClass {
  public static void main(String[] args) {
    List<String> stringList = new ArrayList<>(); // 1
    stringList.add("localhost:8080"); // 2
    stringList.add("localhost:3000"); // 3
    addString(stringList); // 4
  }
  
  private static void addString(List<String> list) {
		list.add("new localhost!"); // 5
  }
}
```

코드 `1` : `new` 연산자를 통해 List\<String>타입의 `stringList`를 동적으로 생성한다.

>##### Stack = [stringList]
>
>stringList -> List<>
>
>##### Heap = [List<>]

코드 `2` : `stringList`에 "localhost:8080"이라는 String을 추가하였다.
이때, "localhost:8080"가 Heap 영역에  할당된다.

>##### Stack = [stringList]
>
>stringList -> List<>
>
>##### Heap = [
>
>##### 	List<0>, 
>
>##### 	"String | localhost:8080"
>
>##### ]
>
>List<0> -> "String | localhost:8080"

코드 `3` : `stringList`에 "localhost:3000"이라는 String을 추가하였다.
이때, "localhost:3000"가 Heap 영역에  할당된다.

>##### Stack = [stringList]
>
>stringList -> List<>
>
>##### Heap = [
>
>##### 	List<0, 1>, 
>
>##### 	"String | localhost:8080", "String | localhost:3000"
>
>##### ]
>
>List<0> -> "String | localhost:8080"
>
>List<1> -> "String | localhost:3000"

코드 `4` : `addString()`을 호출하여 참조변수 `stringList` 를 인자로 넘겨주었다.
메서드가 실행되면서 Stack의 scope가 변경된다. 참조값이 복사된 파라미터 `list` 는 Heap영역의 `List<>` 를 참조하고 있을 것이다.

>##### Stack = [~~stringList~~, list]
>
>~~stringList -> List<>~~
>
>list -> List<>
>
>##### Heap = [
>
>##### 	List<0, 1>, 
>
>##### 	"String | localhost:8080", "String | localhost:3000"
>
>##### ]
>
>List<0> -> "String | localhost:8080"
>
>List<1> -> "String | localhost:3000"

코드 `5` : `list.add()` 를 통해 공통으로 가르키는 Heap영역의 List<>에 스트링을 추가한다.

>##### Stack = [~~stringList~~, list]
>
>~~stringList -> List<>~~
>
>list -> List<>
>
>##### Heap = [
>
>##### 	List<0, 1, 2>, 
>
>##### 	"String | localhost:8080", "String | localhost:3000", "String | new localhost!"
>
>##### ]
>
>List<0> -> "String | localhost:8080"
>
>List<1> -> "String | localhost:3000"
>
>List<2> -> "String | new localhost!"

이제 메서드가 종료되면서 Stack의 scope도 변경되었고, Stack에서는 `list`의 `pop`이 일어나고, main Thread도 종료 되면서 `List<>` 를 참조하는 `stringList` 도 `pop`이 일어날 것이다. 그렇다면 `List<>` 를 참조하는 변수는 더 이상 존재하지 않고 이를 `Unreachalbe 오브젝트`라고 불린다. 이러한 오브젝트에 대해 GC(가비지 컬렉터)가 Heap영역에서 우선적으로 제거하여 메모리 공간을 확보한다.

만약 `stringList`가 `pop`되지 않고 다른 `List<>` 를 참조하는 경우에도, GC가 기존에 참조되고 있던 `List<>` 를 제거한다.



- Heap은 Stack과 다르게 메모리의 제약이 덜 하다. GC가 존재하여 불필요한 메모리 할당에 대해서도 해결을 해준다. 하지만, 모든 Thread가 공유한다는 점을 고려하여 동시성에 대한 이슈에 대해서도 인지하고 있어야 한다.

### Method area

- Thread의 수와 관계 없이 하나만 존재하며, 모든 Thread가 공유한다.
- 클래스나 인터페이스에 대한 메타데이터 정보가 저장되는 공간이다.
  `type, field, method, constant pool, static variable`
- Java 파일에서 전역 변수와 **static으로 선언된** 메서드나 변수가 저장되는 공간이다.

Static 영역의 데이터는 프로그램의 시작부터 종료가 될 때 까지 메모리에 남아있기 때문에 `static`을 필요에 맞게 사용하는 것이 좋다.

```java
public class StaticAreaClass {
  public static int TEN = 10;
}
```

```java
public class PlayGround {
  public void play() {
    System.out.println(StaticAreaClass.TEN);
  }
}
```

`StaticAreaClass`의 `static` 변수 `TEN`은 `new`연산자를 통해 생성하지 않아도 Method area에 저장되어 있기 때문에 사용할 수 있다.


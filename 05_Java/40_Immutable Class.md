# Immutable Class

### Immutable Class?

> - 불변 클래스
>
> 클래스의 상태를 변경할 수 없는 클래스라는 뜻.

- 장점
  클래스의 상태가 변경되지 않는다면 안전성을 보장 받을 수 있다. 조작되어선 안될 상태(비밀번호, url 등)에 적합할 수 있고, 변경에 대한 걱정을 하지 않아도 되기 때문에 해당 객체를 공유하여 사용할때도 비교적 안전하다고 할 수 있다. 또, 복사나 비교를 위한 조작이 단순해지고, 성능 개선에 큰 기여를 할 수 있다.
- 단점
  만약, 변경이 자주 일어나는 경우에 대해서는 기대하는 성능 향상을 얻을 수 없다. 매번 새로운 메모리를 할당하고 새로 생성하면서 점점 차지하는 메모리 영역이 증가하기 때문이다.



### final

final 키워드가 보통 상수를 선언할 때 사용한다. 프로그래밍에서 사용하는 상수란, 변경되지 않는 변수를 의미하는데, 불변성과 정확히 일치한다. final 변수는 변경이 불가능 한 것이지, 할당이 불가능한 것은 아니다.

변수 뿐만아니라 `Class`나 `Method` 혹은 `method`의 인자 값에도 `final` 키워드를 붙힐 수 있다.

- `final Class` : 상속이 불가능한 클래스
- `final method` : 오버라이드가 불가능한 메서드
- `final parameter` : 인자로 받아온 값은 변경될 수 없다는 의미



### Java에서의 불변 클래스

Java에서 자주 사용되는 클래스 중에도 불변 클래스들이 존재한다.

대표적으로, `String` 이 있다.

```java
public class playGround {

    public void play() {
        String a = "A";
        changeStr(a);
        System.out.println("a = " + a);
    }

    private void changeStr(String strA) {
        strA += "B";
        System.out.println("strA = " + strA);
    }
}
//str = AB
//a = A
```

위 코드에서 살펴보면, `changeStr`메소드로 `a` 라는 `String객체` 를 보냈다. Heap영역에 `"A"`를 참조하는 참조값을 `strA`로 복사하고, `"B"`라는 문자를 더했다. 출력해보면 `"AB"`가 나옴을 알 수 있다.

하지만 메서드가 종료되고, 다시 `a` 를 출력해보니 `"A"`만 나옴을 알 수 있다.



```java
public class playGround {

    public void play() {
        String a = "A";
        System.out.println(a.hashCode());  // 65
        changeStr(a);
    }

    private void changeStr(String str) {
        System.out.println(str.hashCode()); // 65
        str += "B";
        System.out.println(str.hashCode()); // 2081
    }
}
```

`String` 은 불변객체이기 때문에 `strA += "B";`해당 코드가 수행되면, `str` 이 참조하고 있는 Heap영역의 `"A"` 에 `"AB"`가 수행되는것이 아니라, 새로운 `"AB"`라는 오브젝트를 만들고 `str` 이 이름 참조하도록 한다. 그래서 `"B"` 를 더한 뒤 `hashCode()`가 달라진것을 확인할 수 있다.

만약, 문자열에 대한 조작이 많이 필요하다면 그 조작 과정 하나하나 마다 `String` 객체가 생성되어 메모리를 차지하고 있을 것이다. 물론 Heap영역에서 GC가 제거하고 있지만, `Unreachable` 이 되기 전까지 많은 조작들이 이루어진다면 성능 저하를 초래할 수 있다. 이러한 상황에 적합한 문자열 Class가 `StringBuilder`인 것이다

### StringBulider

```java
public class playGround {

    public void play() {
        StringBuilder a = new StringBuilder("A"); 
        System.out.println(a.hashCode()); //1057941451
        changeStr(a);
	      System.out.println("a = " + a); //AB
    }

    private void changeStr(StringBuilder str) {
        System.out.println(str.hashCode()); //1057941451
        str.append("B");
        System.out.println(str.hashCode()); //1057941451
    }
}
```

위와 동일한 코드에서 `String -> StringBuilder`로 변경하여 작성하였다.

이젠 `"A"`에 `"B"`를 더하는 로직을 수행하여도 같은 `hashCode()`를 가진다는 것을 확인할 수 있다.

### StringBuffer

`StringBuffer`도 `StringBuilder`거의 유사하게 `mutable`하게 작동한다. 차이점이라고 한다면 멀티 스레드환경에서의 안정성 정도이다.

### String 3형제를 정리해보면,

- 읽기용이나 공유용도로 문자열을 사용해야 한다. -> String
   (단일 문자열 참조일 경우)

- 문자열 수정을 해야 하면서 멀티 스레드 환경이다. -> StringBuffer

  (동기화 기능 O -> 멀티 스레드 환경에서 안정성 보장)

- 문자열 수정을 해야 하면서 싱글 스레드 환경이다. -> StringBuffer

  (동기화 기능 X -> 싱글 스레드 환경에서 사용 권장)
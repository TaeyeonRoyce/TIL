# JUnit 5 Annotations



### 테스트 케이스

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class MyFirstJUnitJupiterTests {

    private final Calculator calculator = new Calculator();

    @Test
    void addition() {
        assertEquals(2, calculator.add(1, 1));
    }

}
// from junit.org
```

위 코드에서 `@Test`, `.assertEquals()`와 같은 부분이 Junit의 존재이유이다.

간단히, `@Test`는 이 메소드가 Test메소드라는 걸 알리는 어노테이션으로, Junit 모듈이 이를 찾아 실행 할 것이다.

`.assertEquals()`는 Junit이 제공하는 메소드로 일치를 확인할 수 있다.

### @Test


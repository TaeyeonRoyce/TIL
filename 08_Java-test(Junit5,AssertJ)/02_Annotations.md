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

- 테스트 코드임을 나타내는 Annotation

- JUnit은 각각의 테스트가 서로 영향을 주지 않고 독립적으로 실행됨을
  원칙으로 하므로 @Test 마다 객체를 생성한다.

```java
@Test
void addition() {
  assertEquals(2, calculator.add(1, 1));
}
```



### @Ignore

- 테스트를 실행하지 않는다는 Annotation



### @ BeforeEach(JUnit 5), @Before(JUnit 4)

- `@Test`메소드가 실행되기 전에 먼저 실행되는 메소드를 나타내는 Annotation

```java
class Calculator {
	public int add(int a, int b) {
		return a + b;
	}
}

public class Test {
	int a;
	Calculator calculator = new Calculator();

	@BeforeEach
	public void setA() {
		this.a = 2;
	}

	@Test
	public void additionTest_1() {
		assertEquals(a, calculator.add(1, 1));
		this.a = 5;
	}

	@Test
	public void additionTest_2() {
		assertEquals(a, calculator.add(0, 2));
	}
}
/*
setA 				  		a -> 2
additionTest_1  	a -> 5
setA    		  		a -> 2
additionTest_2    
*/
```



### @BeforeAll(JUnit 5), @BeforeClass(JUnit 4), 

- `@Test`메소드가 실행되기 전에 먼저 한 번만 실행되는 메소드를 나타내는 Annotation
- *static*으로 선언해야함!



### @ AfterEach(JUnit 5), @After(JUnit 4),

- `@Test`메소드가 실행되고 나서 실행되는 메소드를 나타내는 Annotation

```java
class Calculator {
	public int add(int a, int b) {
		return a + b;
	}
}

public class TEstsa {
	int a;
	Calculator calculator = new Calculator();

	@AfterEach
	public void printSuccess() {
		System.out.println("SUCCESS");
	}

	@Test
	public void additionTest_1() {
		assertEquals(2, calculator.add(1, 1));
	}

	@Test
	public void additionTest_2() {
		assertEquals(2, calculator.add(0, 2));
	}
}
/*
additionTest_1
SUCCESS
additionTest_2    
SUCCESS
*/
```



### @AfterClass

- `@Test`메소드가 모두 수행된 후에 마지막에 수행됨
- *static*으로 선언해야함!



### @DisplayName

- 테스트 클래스나 메소드의 이름을 선언할 때 사용

```java
@DisplayName("테스트_코드_1")
@Test
public void test() {
  ...
}
```


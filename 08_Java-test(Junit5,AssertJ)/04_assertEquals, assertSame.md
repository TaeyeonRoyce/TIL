# assertEquals()

```java
import org.junit.jupiter.api.Assertions;


assertEquals(int expected, int actual, String message?)
//int타입 말고, byte, short, long, char, Object가 올 수 있음
  
assertEquals(float expected, float actual, float delta?, String message?)
assertEquals(double expected, double actual, double delta?, String message?)
//?는 옵셔널
```

- expected, actual :

  actual의 값이 expected와 동일한지 체크한다.

- delta :

  실수간의 비교에서, delta보다 작은 차이는 같은 값으로 간주한다.

- message :

  테스트 결과가 실패일 때, 화면에 출력될 메시지

> 기본 사용

```java
@Test
	public void Test_1() {
		int expectedNum = 100;
		Assertions.assertEquals(expectedNum, 100);
	}
//PASS
```



> delta의 값을 참고하여 expectedNum과 100을 동일하게 본다.

```java
@Test
	public void Test_2() {
		double expectedNum = 100.00001;
		Assertions.assertEquals(expectedNum, 100, 0.001);
	}
//PASS
```



> String도 비교가 가능하다. 기본적으로 equals를 호출하여 비교한다

```java
@Test
	public void Test_3() {
		String A_1 = "A";
		String A_2 = "A";
		Assertions.assertEquals(A_1, A_2);
	}
//PASS
```



> 객체 또한 equals를 호출하기 때문에 아래 test는 통과하지 못한다.

```java
class Person {
	String name;
	public Person(String name) {
		this.name = name;
	}
}

public class PlayGround{
  @Test
    public void Test_4() {
      Person person1 = new Person("Me");
      Person person2 = new Person("Me");
      Assertions.assertEquals("객체 주소로 비교합니다..",person1, person2);
    }
  //org.opentest4j.AssertionFailedError: 객체 주소로 비교합니다.. ==> ...
}
```



> 아래 결과를 보면 equals를 호출 한다는 것을 알 수 있다.

```java
class Person {
	String name;

	public Person(String name) {
		this.name = name;
	}

	@Override
	public boolean equals(Object o) {
		if (o instanceof Person) {
			return this.name == ((Person)o).name;
		}
		return false;
	}
}

public class PlayGround{
  @Test
    public void Test_4() {
      Person person1 = new Person("Me");
      Person person2 = new Person("Me");
      Assertions.assertEquals(person1, person2,"객체의 이름으로 비교합니다");
    }
}
//PASS
```



> assertNotEquals도 존재한다.
>
> 사용방법은 동일하며 메서드 이름에서 알 수 있듯이,
>
> assetEquals가 거짓인 테스트에 대해 참을 반환한다.
>
> ```java
> public class PlayGround {
> 	@Test
> 	public void Test_1() {
> 		String A_1 = "A";
> 		String A_2 = "B";
> 		Assertions.assertNotEquals(A_1, A_2);
> 	}
> }
> //PASS
> ```



# assertSame()

```java
import org.junit.jupiter.api.Assertions;

assertSame(Object expected, Object actual, String message?)
//?는 옵셔널
```

- expected, actual :

  actual의 참조변수가 expected의 참조변수와 동일한지 체크한다.

  

- message :

  테스트 결과가 실패일 때, 화면에 출력될 메시지

> assertEquals와 거의 유사하지만, 아래의 코드를 통해 참조변수의 비교가 필요한 이유를 알 수 있다.
>
> (주로 *equals*가 Override된 경우)

```java
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

class Person {
	String name;

	public Person(String name) {
		this.name = name;
	}

	@Override
	public boolean equals(Object o) {
		if (o instanceof Person) {
			return this.name == ((Person)o).name;
		}
		return false;
	}
}

public class PlayGround{
	Person person1 = new Person("Me");
	Person person2 = new Person("Me");
	@Test
	public void Test_1() {
		Assertions.assertEquals(person1, person2,"객체의 이름으로 비교합니다.."); //PASS
	}

	@Test
	public void Test_2() {
		Assertions.assertNotSame(person1, person2,"객체 주소로 비교합니다.."); //PASS
	}
}
```



### ++ 참고

```java
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

public class PlayGround{
	@Test
	public void Test_1() {
		Assertions.assertSame(100, 100); //PASS
	}

	@Test
	public void Test_2() {
		Assertions.assertNotSame(130, 130); //PASS
	}
}
```

같은 int(기본형)타입의 값을 넣고 assertSame/assertNotSame을 수행했는데 모두 통과가 되었다.

java는 기본적으로 `-127~127`까지의 정수를 생성해두기 때문에 그 사이의 값은 새로운 객체 생성이 되지 않기 때문에 같은 참조변수를 가진다.
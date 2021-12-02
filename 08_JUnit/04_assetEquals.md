# assertEquals()

```java
import org.junit.jupiter.api.Assertions;

assertEquals(expected, actual, delta?, String message?)
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
      Assertions.assertEquals(person1, person2,"객체 주소로 비교합니다..");
    }
}
//PASS
```



# AssertJ

### AssertJ는 더 다양한 assertion을 제공하는 자바 라이브러리이다.

> 공식 문서 : https://assertj.github.io/doc/

-  특징

> 메서드 체이닝을 지원하여 더 직관적이고 읽기 쉬운 테스트코드를 작성,

> 정확한 에러메세지

> JUnit이외의 추가적인 메서드 지원

> BDD 스타일 어센션 지향



*BDD 스타일 어센션*

- BDD (Behavior Driven Development)는 **"행동"**을 기준으로 하는 개발 방법론

  >given
  >
  >when
  >
  >then
  >
  >을 기억하자... 추후 정리



### AssertJ 호출하기

```java
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

//import static org.assertj.core.api.Assertions.*; 정적 임포트
  
public class TestPlayGround {
	@Test
	public void Test_1() {
		Assertions.assertThat()
	}
}
```



### assertThat

AssertJ의 모든 테스트 코드는 assertThat으로 시작한다.

```
assertThat(테스트 타겟).메소드1().메소드2().메소드3()
```

이런 포맷으로 AssertJ의 여러 메소드들을 연쇄적으로 호출해 코드를 작성할 수 있다.
(메서드 체이닝)



### 간단한 예시

```java
import static org.assertj.core.api.Assertions.assertThat; 

import org.junit.jupiter.api.Test;

public class SimpleAssertionsExample {
  @Test
  void a_few_simple_assertions() {
    assertThat("The Lord of the Rings").isNotNull()   
                                       .startsWith("The") 
                                       .contains("Lord") 
                                       .endsWith("Rings"); 
  }
}
//ref - "https://assertj.github.io/doc/"
```

코드를 살펴보면, assertThat을 시작으로 "The Lord of the Rings" 에 대해서, 

 `.isNotNull()  ` : Null이 아닌지,

`.startsWith("The")` : "The"로 시작하는지,

 `.contains("Lord") ` : "Lord"를 포함 하는지,

` .endsWith("Rings")` : "Rings"로 끝나는지

의 메소드들이 체이닝 되어 실행한다.

설명하지 않아도 직관적으로 어떤 테스트를 하는 지 알 수 있다.



### AS

실행될 assertion에 대한 설명을 추가할 수 있다.

```java
import static org.assertj.core.api.Assertions.*;

import org.junit.jupiter.api.Test;

class Person {
	private String name;
	private int age;

	public String getName() {return name;}
	public void setName(String name) {this.name = name;}
	public int getAge() {return age;}
	public void setAge(int age) {this.age = age;}
}

public class TestPlayGround {
	@Test
	public void Test_1() {
		//given
		Person person = new Person();

		//when
		person.setName("Royce");
		person.setAge(23);

		//then
		assertThat(person.getAge()).as("check %s's age",person.getName()).isEqualTo("23");
	}
}

/*
[check Royce's age]  
expected: "23"
 but was: 23
 */
```

`as("check %s's age",person.getName())` 를 통해 테스트에 대한 설명을 추가하였다/

실패한 테스트에 대하여 `[check Royce's age] ` 와 같은 문구를 출력하였다.

`as`는 `assertion`이전에 불러야 한다.



### .filteredOn

java의 기본문법인 람다와 사용방식이 유사하다.

```java
public class TestPlayGround {
	@Test
	public void Test_1() {
		//given
		ArrayList<Person> people = new ArrayList<>();
		Person royce_1 = new Person("Royce_1", 20);
		Person royce_2 = new Person("Royce_2", 32);
		//when
		people.add(royce_1);
		people.add(royce_2);

		//then
		assertThat(people).filteredOn(person -> person.getAge() < 30).containsOnly(royce_1);
	}
}
```

```java
public class TestPlayGround {
	@Test
	public void Test_1() {
		//given
		ArrayList<Person> people = new ArrayList<>();
		Person royce_1 = new Person("Royce_1", 20);
		Person royce_2 = new Person("Royce_2", 32);
		//when
		people.add(royce_1);
		people.add(royce_2);

		//then
		assertThat(people).filteredOn("age", 32).containsOnly(royce_2);
	}
}
```

객체의 프로퍼티에도 직접 접근할 수 있다. 위 코드에서 `age`라는 인스턴트 변수는 `private`인데도 Test가 가능하다.

### .extracting

```java
public class TestPlayGround {
	@Test
	public void Test_1() {
		//given
		ArrayList<Person> people = new ArrayList<>();

		//when
		people.add(new Person("Royce_1", 20));
		people.add(new Person("Royce_2", 32));
		people.add(new Person("Royce_3", 32));
		people.add(new Person("Royce_4", 32));

		//then
		assertThat(people).extracting("name").contains("Royce_1");
	}
}
```

`extracting("name")`의 결과는

```java
Expecting ArrayList:
  ["Royce_1", "Royce_2", "Royce_3", "Royce_4"]
```

이다. 이렇게 객체 뿐만 아니라, 객체의 프로퍼티에 대해서도 쉽게 제어 및 조작이 가능하다.

> contains나 containsOnly와 같은 Collection과 배열에 대한 검증 메서드의 종류가 더 많이 존재 한다.
>
> Iterable and array assertions : https://assertj.github.io/doc/#assertj-core-group-assertions



## Exception 처리

`assertThatThrownBy()`

```java
public class TestPlayGround {
	@Test
	public void Test_1() {
		assertThatThrownBy(() -> {throw new Exception("boom!");})
			.isInstanceOf(Exception.class)
			.hasMessageContaining("boom");
	}
}
```

또 다른 방법 `asserThatExcetionOfType()`

```java
public class TestPlayGround {
	@Test
	public void Test_1() {
		assertThatExceptionOfType(Exception.class)
			.isThrownBy(() -> {
				throw new Exception("boom");})
			.withMessage("boom");
	}
}
```



자주 발생하는 예외에 대해선 특정 메서드를 제공한다

- `assertThatNullPointerException()`
- `assertThatIllegalArgumentException()`
- `assertThatIllegalStateException()`
- `assertThatIOException()`

```java
public class TestPlayGround {
	@Test
	public void Test_1() {
		assertThatIOException()
			.isThrownBy(() -> {
				throw new IOException("boom");})
			.withMessage("boom");
	}
}
```

```java
public class TestPlayGround {
	@Test
	public void Test_1() {
		assertThatNullPointerException()
			.isThrownBy(() -> {
				throw new NullPointerException("boom");})
			.withMessage("boom");
	}
}
```



예외가 발생하지 않는 경우

`assertThatNoException()`

```java
public class TestPlayGround {
	@Test
	public void Test_1() {
		assertThatNoException().isThrownBy(() -> System.out.println("OK"));
	}
}
```








# Bean Lifecycle

`Spring Container`는 `Bean`객체들을 관리한다. 여기서 관리란 `Bean`객체의 생성과 소멸, 의존성 주입등을 담당하는데, 이를 **생명주기(Lifecycle)**를 관리한다고 한다.



`Spring Container`가 `Bean`의 생명주기를 관리하는 흐름에 대해 간단히 살펴보면,

1. `Spring Container`가 생성된다.
2. 각각의 요청에 맞는 `Bean`의 인스턴스들이 생성된다.이때, `.xml`파일, `config.class`, `@Annotation`으로 설정된 `Bean`이다.
3. 의존관계가 주입(Dependency Injection)이 이뤄진다. 
4. 생성된 `Bean`들의 초기화(Initialize) 콜백 메서드가 호출된다.
5. 어플리케이션에서 `Bean`들이 사용된다.
6. 생성된 `Bean`들의 소멸(Destory) 콜백 메서드가 호출된다.
   (`Spring Container`가 종료되기 직전에 소멸 시점을 알려준다고 함)
7. `Spring`종료.

일반적으로 예상 가능한 `Lifecycle` 인것 같다. 컨테이너의 생성과 `Bean`을 사용하기 위해 등록(생성)하고 사용하고 소멸되는 단계들이기 때문이다. 하지만 초기화와 소멸 콜백 메서드들이 존재한다. **보통의 객체들 처럼 생성자를 사용해서 초기화하지 않고 분리한 이유가 있을까?**

소켓 통신이나 DB의 `Connection Pool`과 같은 무거운 작업을 수행하는 경우에는 분리하여 작업하는 권장한다고 한다.*

## Bean 초기화/소멸

### InitializingBean, DisposableBean

`Spring`에서 제공되는 `Interface`로 `Bean`에서 구현하면 된다.

초기화 콜백은 `afterPropertiesSet()`메서드가, 소멸 콜백은 `destroy()` 메서드를 오버라이딩하여 설정할 수 있다.

### @TestBean

```java
public class TestBean implements InitializingBean, DisposableBean {
	String name;

  //생성자
	public TestBean() {
		printName();
	}

	public void setName(String name) {
		this.name = name;
	}

	public void printName() {
		System.out.println("hello = " + name);
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("TestBean이 초기화 된다.");
    printName();
	}

	@Override
	public void destroy() throws Exception {
		System.out.println("TestBean이 소멸된다.");
	}

}

```



### Test

```java
@Test
	public void Test() {
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(
			TestConfig.class);

		ac.getBean(TestBean.class);

		ac.close();
	}

//TestBean등록 config
	@Configuration
	static class TestConfig {
		@Bean
		public TestBean testBean(){
			TestBean testBean = new TestBean();
			testBean.setName("Royce");
			return testBean;
		}
	}
```



### 출력 결과

```
hello = null
TestBean이 초기화 된다.
hello = Royce
00:54:10.674 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@47e2e487, started on Thu Mar 03 00:54:10 KST 2022
TestBean이 소멸된다.
```

출력결과를 살펴보자.

첫 출력결과에서는 생성자가 수행된다. `new` 연산자로 `TestBean()`이 생성되면서 필드인 `name`을 출력했고 `null`임을 확인 할 수 있다.

두번째 출력결과인 "TestBean이 초기화 된다."와 함께 `name`에 `Royce`가 들어간 것을 확인 할 수 있다.
이를 통해 초기화콜백 메서드가 `Bean`등록 이후 이루어짐을 알 수 있다.

그후 `ac.close()`가 수행 된 뒤, `destroy()`가 수행되었다.

`afterPropertiesSet()`와 `destroy()` 메서드는 직접 호출하지 않았는데도 각각의 생명주기에 맞춰 수행되었음을 확인 할 수 있다.



### 단점

예시로 사용한 `InitializingBean`,` DisposableBean ` interface는 권장하지 않는다.

- `Spring`전용 `interfcace`여서 `Spring`의존도가 높다.
- `Override`를 하다 보니 메서드의 이름을 변경할 수 없다.
- 외부 라이브러리에 메서드를 제공할 방법이 없다. (외부 라이브러리에서 이 인터페이스를 구현할 수 없다.)



### @Bean 설정으로 초기화/소멸 콜백 사용

`@Bean(initMethod = "{초기화 메서드}", destroyMethod = "{소멸 메서드}")` 형식을 통해 사용할 수 있다.

아까 예제를 사용해 보자.

### @TestBean

```java
public class TestBean {
	String name;

  //생성자
	public TestBean() {
		printName();
	}

	public void setName(String name) {
		this.name = name;
	}

	public void printName() {
		System.out.println("hello = " + name);
	}

	public void initTestBean() {
		System.out.println("TestBean이 초기화 된다.");
    printName();
	}

	public void destroyTestBean() {
		System.out.println("TestBean이 소멸된다.");
	}

}

```



### Test

```java
@Test
	public void Test() {
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(
			TestConfig.class);

		ac.getBean(TestBean.class);

		ac.close();
	}

//TestBean등록 config
	@Configuration
	static class TestConfig {
    //destroyMethod는 생략 가능하다.
		@Bean(initMethod = "initTestBean", destroyMethod = "destroyTestBean")
		public TestBean testBean(){
			TestBean testBean = new TestBean();
			testBean.setName("Royce");
			return testBean;
		}
	}
```



### 출력결과

```
hello = null
TestBean이 초기화 된다.
hello = Royce
01:09:22.248 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@47e2e487, started on Thu Mar 03 01:09:21 KST 2022
TestBean이 소멸된다.
```

동일하게 작동함을 알 수 있다.



### @PostConstruct, @PreDestory

마지막으로 가장 많이 쓰이는 방법을 살펴 보자.

`Annotation`으로 초기화 메서드 위에는 `@PostConstruct`, 소멸 메서드 위에는 `@PreDestory`를 붙히면 된다.

### @TestBean

```java
public class TestBean {
	String name;

  //생성자
	public TestBean() {
		printName();
	}

	public void setName(String name) {
		this.name = name;
	}

	public void printName() {
		System.out.println("hello = " + name);
	}

  @PostConstruct
	public void initTestBean() {
		System.out.println("TestBean이 초기화 된다.");
    printName();
	}

  @PreDestroy
	public void destroyTestBean() {
		System.out.println("TestBean이 소멸된다.");
	}

}

```



### Test

```java
@Test
	public void Test() {
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(
			TestConfig.class);

		ac.getBean(TestBean.class);

		ac.close();
	}

//TestBean등록 config
	@Configuration
	static class TestConfig {
		@Bean
		public TestBean testBean(){
			TestBean testBean = new TestBean();
			testBean.setName("Royce");
			return testBean;
		}
	}
```

### 출력결과

```
hello = null
TestBean이 초기화 된다.
hello = Royce
01:09:22.248 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@47e2e487, started on Thu Mar 03 01:09:21 KST 2022
TestBean이 소멸된다.
```

동일하게 작동!

이 어노테이션은 `javax`패키지에 존재하는 `Java`기술이기에 `Spring`에 의존적이진 않다. 하지만 이 방법도 외부 라이브러리에는 적용할 수 없다고 한다.

`Bean Lifecycle`에 대해 살펴보고, 초기화/소멸 메서드를 다루는 과정을 살펴보았다.
결론적으론, `@PostControuct`, `@PreDestory`전략이 가장 권장되는 것 같다.
외부 라이브러리를 사용하는 경우에는 어쩔 수 없이 `@Bean`옵션을 주는것이 좋은 선택인 것 같다.



### 🔎 더 알아볼 내용 

- `DI`와 `IoC`
- `AnnotationConfigApplicationContext.close()`가 필수인가?

### ❓의문점

- 생성자를 활용한 초기화와 그냥 초기화를 하면 어떤 이점이 있는지.
- 초기화 메서드에서 다른`Bean`을 참조할 수 있을까?

# @Bean



## Bean이란?

- `IoC Container`가 관리하는 자바 객체

`Spring`에서는 `Container`라는 개념을 통해 인스턴스의 생명주기를 관리하고 추가적인 기능을 제공한다. 이때, `Container`는 개발자가 직접 수행했어야 할 몇가지 제어권을 가지는데, 작성한 코드를 바탕으로 스스로 참조한 뒤 알아서 객체를 생성 및 소멸하는 역할을 수행해준다.

> `Pet dog = new Pet()` 처럼 직접 작성하지 않고,
>
> `Pet dog;`라는 코드만 남겨두면, Spring IoC Container가 제어권을 역전받아 참조하는(의존하는) 객체를 스스로 생성하고 참조시킨다. 
>
> `IoC Container`에 대한 개념은 추후 자세히 다루도록 하자.

이런 역할을 하는 `IoC Container`에 의해 생성되고 관리되는 객체를 `Bean`이라고 부른다



## Bean

일반적으로 `Bean`은 기본적으로 싱글톤 패턴으로 존재한다.

>*싱글톤 패턴 : 어떤 Class가 최초 한번만 생성되는 디자인 패턴*
>
>@Scope("singletone") default
>
>@Scope("prototype")
>Bean을 요청이 될 때 마다 새로운 객체를 생성하도록 함.

`Bean`은 `Spring Container`에서 생성되고 관리되는 객체이지만 코드 내에서 흔히 생성되는 방식인 `new`연산자로 생성되진 않는다. 

> Container에 등록된 Bean객체를 얻고자 할 때는 ApplcationContext.getBean()을 통해 얻을 수 있다.

## Bean의 생성과 등록

## Annotation

`IoC Container`는 `@Component`가 붙어있는 모든 Class에 대해서 인스턴스를 생성해 `Bean`으로 등록한다.

`@Component`는 `@ComponentScan`이 붙어있는 Class가  해당 작업을 수행하는데, 해당 Class하위 패키지에에 존재하는 `@Component`에 대해서만 수행한다.



### @SpringBootApplication

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
}
```

`@SpringBootApplication`은 Spring 프로젝트를 생성하면 기본적을 생성되는 main클래스(main메서드를 포함한)에 붙어있다.
`@ComponentScan` 어노테이션이 붙어있는 것을 확인 할 수 있다. 

자주 사용하는 Controller - Service - Repository Layer에서 보면 우리는 `@Component`라는 어노테이션을 붙히지 않지만 Container로 부터 의존성 주입을 받는다. 그게 가능한 이유는 `@Controller`, `@Service`, `@Repository`어노테이션이 이미 `@Component`를 포함하고 있기 때문이다.

### @Controller

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {
}
```

### @Service

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Service {
}
```

### @Repository

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {
}

```

이렇게 등록할 Class에 `@Component` 어노테이션을 붙히면 `@ComponentScan`이 붙어있는 Class가 해당 클래스가 있는 패키지를 포함하여 하위 패키지의 모든 `@Component`를 등록해준다.

## @Configuration, @Bean정의

Java Class에서 `@Configuration` 어노테이션을 사용하여 직접 `@Bean`을 등록하는 방법이다.

`@Configuration`도 내부적으로 `@Component`어노테이션을 가지기 때문에 `@Bean`을 포함한 메서드들도 등록이 된다.

```java
@Configuration
public class TestConfiguration {
	@Bean
	@Scope("prototype")
	public PrototypeBean prototypeBean() {
		return new PrototypeBean();
	}
	@Bean
	public SingleToneBean singleToneBean() {
		return new SingleToneBean();
	}
}
```

위 코드를 보면 `@Bean` 어노테이션이 붙은 메서드는 `IoC Container`에 `key : method name, value : return value`으로 등록된다. 적용해 보면, 
`"prototypeBean" : new PrototypeBean(), "singleToneBean"  : new SingleToneBean()` 이다.

# Bean 조회

### TestBean

```java
public class TestBean {
	int number = 10;

	public void add() {
		this.number += 10;
	}
}
```

### TestConfiguration

```java
@Configuration
public class TestConfiguration {
	@Bean //싱글톤(기본)으로 반환
	public TestBean singleToneBean() {
		return new TestBean();
	}
  
  @Bean
	@Scope("prototype") //싱글톤이 아닌 패턴으로 반환
	public TestBean prototypeBean() {
		return new TestBean();
	}
}
```

### Test

```java
@Test
public void Test() {
  ApplicationContext ac = new AnnotationConfigApplicationContext(
    TestConfiguration.class);

  //key = "singleToneBean"으로 get
  TestBean singleToneBean1 = (TestBean)ac.getBean("singleToneBean"); 
  TestBean singleToneBean2 = (TestBean)ac.getBean("singleToneBean");
  singleToneBean1.add();
  
  //key = "prototypeBean"으로 get
  TestBean prototype1 = (TestBean)ac.getBean("prototypeBean");
  TestBean prototype2 = (TestBean)ac.getBean("prototypeBean");
  prototype1.add();

  assertThat(prototype1.number).isNotEqualTo(prototype2.number);
  assertThat(singleToneBean1.number).isEqualTo(singleToneBean2.number); //싱글톤이라 동일하게 적용
}
```



### 🔎 더 알아볼 내용 

- `@ComponentScan`

- Bean조회에 대한 개념 학습 필요
  @Component로 등록한 Bean에 대한 조회시 싱글톤/싱글톤X 구분 가능 로직에 대한 어려움을 겪음

- ` ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfiguration.class);`
  Container처럼 사용되는 것 같은데 이에 대해서도 알아보자.

- `IoC Container`의 `Life Cycle`

  

### ❓의문점

- 싱글톤으로 관리되는 Bean들에 대한 동시성 이슈는 어떻게 관리하는지
- `@Componenet` 와 `@Bean` 으로 등록하는 방식의 차이점


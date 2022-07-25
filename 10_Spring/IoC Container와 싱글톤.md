# Singleton (싱글톤)

Enterprise급을 위해 탄생한 Spring에선, `IoC Container`에 등록되는 객체(빈)을 싱글톤으로 관리한다.

> [싱글톤에 대해 정리해 둔 내용](https://velog.io/@roycewon/Singleton-싱글톤-패턴)

서버에 들어오는 수 많은 요청들에 대해서, 항상 새로운 객체를 생성해서 (new 연산자) 관리를 한다면 굉장히 많은 메모리를 차지 할 것이기 때문이다.
싱글톤 패턴의 문제점들은 다양하다.

- 싱글톤 패턴을 구현하는 코드 자체가 많이 들어간다.
- 의존관계상 클라이언트가 구체 클래스에 의존한다. (DIP 위반)
- 클라이언트가 구체 클래스에 의존해서 OCP 원칙을 위반할 가능성이 높다.
- 테스트하기 어렵다.

종합적으로 유연성이 떨어진다는 특징을 가진다.

Spring Container는 이러한 문제점을 해결하고, 객체들을 싱글톤으로 관리한다.

```java
@Test
public void configurationTest() {
	ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

	MemberServiceImpl memberService = ac.getBean(MemberServiceImpl.class);
	OrderServiceImpl orderService = ac.getBean(OrderServiceImpl.class);

	MemberRepository memberRepository1 = memberService.getMemberRepository();
	MemberRepository memberRepository2 = orderService.getMemberRepository();
        
    System.out.println("memberRepository1 = " + memberRepository1);
	System.out.println("memberRepository2 = " + memberRepository2);

	assertThat(memberRepository1).isEqualTo(memberRepository);
}
```

위 `MemberServiceImpl`, `OrderServiceImpl`, `MemberRepository` 모두 AppConfig에서 등록한 빈(객체)이다.
Test가 통과하는 것을 보면, Singleton으로 관리한다는 것을 알 수 있다.
![img](https://velog.velcdn.com/images/roycewon/post/f3bedba7-e5e8-475a-8323-6f7823514e7c/image.png)

이 내용을 알아야 하는 이유가 있다.
Spring이 싱글톤으로 빈을 관리한다는 사실을 모른다면, 객체의 상태를 저장할 수도 있기 때문이다. 쉽게 이해하기 위해 다음과 같은 예시를 살펴보자

```java
@Test
public void statefulServiceSingleton() {
    //given
    //StatefulService가 등록되어 있는 TestConfig
	ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);

	StatefulService statefulService1 = ac.getBean(StatefulService.class);
	StatefulService statefulService2 = ac.getBean(StatefulService.class);

	statefulService1.order("userA", 10000);
	statefulService2.order("userB", 20000);

	int price = statefulService1.getPrice();
	assertThat(price).isSameAs(10000);
}
```

StatefulService.class

```java
public class StatefulService {

	private int price;

	public void order(String name, int price) {
		System.out.println("name = " + name + "price = " + price);
		this.price = price;
	}
    
    public int getPrice() {
		return price;
	}
```

Spring Container(TestConfig.class)에 등록된 `StatefulService` 가 존재한다고 하자.
테스트 코드에서는 `StatefulService`를 각각 호출하고, 상태를 저장했다.

- statefulService1 => userA, 10000
- statefulService2 => userB, 20000

 그리고 테스트는 실패한다. 객체를 공유하기 때문이다. `statefulService2.order("userB", 20000);` 를 한다고 다른 객체의 상태를 저장하는 것이 아니기 때문이다.
![img](https://velog.velcdn.com/images/roycewon/post/a2ed4ac4-b322-43b6-b66c-f49e4ea54b9e/image.png)
(친절하게 refer to the same object라고 알려준다)

이 과정을 통해 이해해야 하는 점은 Spring이 기본적으로 객체들을 Singleton 패턴으로 관리하기 때문에 상태 저장하는 것은 위험하다는 점이다.

### SingleTon으로 관리될 수 있는 이유

AppConfig.class

```java
@Configuration
public class AppConfig {

	@Bean
	public MemberService memberService() {
		return new MemberServiceImpl(memberRepository());
	}
	@Bean
	public MemberRepository memberRepository() {
		return new MemoryMemberRepository();
	}
	@Bean
	public OrderService orderService() {
		return new OrderServiceImpl(memberRepository(), discountPolicy());
	}
	@Bean
	public DiscountPolicy discountPolicy() {
		// return new FixDiscountPolicy();
		return new RateDiscountPolicy();
	}
}
```

싱글톤 패턴으로 관리한다기엔, `@Configuration`에서 new 연산자를 자주 사용하고 있지 않는가...? `@Bean`으로 등록된 객체를 싱글톤으로 등록한다면, `orderService()`와 `memberService()`를 등록할 때 이미 `new memberRepository()`를 사용하는데 어떻게 싱글톤이 되는가..

일반적인 Java 코드라면 당연히 안된다.
결론만 말하자면 Spring이 `@Configuration` 어노테이션을 보고 바이트 코드를 조작해서 싱글톤이 보장할 수 있게 다른 객체를 생성해서 저장한다고 한다.

Configuration 어노테이션이 붙은 AppConfig.class를 바탕으로 `AppConfig$$EnhancerBySpringCGLIB$$993824f4@61ce23ac` 이런 클래스를 생성한다는 것이다.

결론은, Spring은 싱글톤 패턴을 통해 많은 요청에 대해 객체를 공유하려고 한다. 그렇기에 객체에 상태를 저장하는 것은 위험하다는 것이다(공유되기 때문).
Spring이 싱글톤 패턴을 유지할 수 있는것은, @Configuration과 같은 설정을 해주었기 때문이다.
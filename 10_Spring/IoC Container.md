# IoC Container

[앞서 살펴본](https://velog.io/@roycewon/DI%EC%99%80-IoC) 것처럼, IoC Container는 오브젝트에 대한 제어권(생성과 관계설정, 사용, 제거 등)을 가지며 대표적으로 의존관계를 주입(DI)을 해주는 역할을 가진다고 살펴보았다.

더 나아가서, 단순한 DI 뿐만 아니라 여러 기능들을 제공하는데 이에 대해 알아보도록 하자.

### ApplicationContext

직접 만든 `AppConfig.class`를 Container처럼 사용 할 때, 우리는 ApplicationContext를 사용했다.
ApplicationContext에 우리가 직접 의존관계를 주입해주는 역할을 가진 `AppConfig.class`를 인자로 하여 생성했더니 자동으로 주입해주지 않았나.
```java
//AppConfig의 구성정보를 바탕으로
//스프링 컨테이너 생성
ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
```
  > `AnnotationConfigApplicationContext`는 `ApplicationContext`를 구현한 클래스

`ApplicationContext`와 같은 스프링 컨테이너가 관리하는 객체를 Bean(빈)이라고 한다.
어플리케이션이 동작할 때, 우리가 직접 설정한 AppConfig을 활용하여 스프링 컨테이너에 빈을 등록한다.
위 예시에서는 `AnnotationConfigApplicationContext` 라는 구현체를 사용했다.

이처럼 Spring Container를 생성하고, 빈을 찾아 등록하는 과정을 마쳤다. 그리고, 엔터프라이즈급 프로그램에서 자주 사용되는 Spring이 이것만 지원할리 없다는 점도 잊지 말아야 한다.

ApplicationContext를 찾아보면, 다음과 같은 정보를 얻을 수 있다.
```java
public interface ApplicationContext extends
	EnvironmentCapable, 
    ListableBeanFactory, 
    HierarchicalBeanFactory, 
    MessageSource, 
    ApplicationEventPublisher, 
    ResourcePatternResolver { //편하게 보기위해 포맷팅을 변경했음
    
    @Nullable
    String getId();
    //...생략
}
```
`ApplicationContext`는 여러 인터페이스를 상속한 인터페이스임을 알 수 있다. 도식화된 자료도 같이 살펴보면,
![](https://velog.velcdn.com/images/roycewon/post/bb150790-5097-4442-a058-28aa64ed01e1/image.png)


`ApplicationContext`가 상속받은 여러 인터페이스들 중에 `BeanFactory`라는 것이 있다. 사실 Spring Container로서의 핵심적인 기능은 `BeanFactory` 인터페이스에 대부분 포함되어 있다고 한다.
> #### BeanFactory
> `BeanFactory`는 스프링 컨테이너의 최상위 인터페이스 이며, 빈을 관리하고 조회하는 역할을 함.

반면에 `ApplicationContext`는 `BeanFactory`뿐만 아니라 `MessgaeSource`, `EnviromentCapable`, `ApplicationEventPublisher`, ... 등 다양한 인터페이스를 추가적으로 상속받고 있다. 기능적인 부분에선 `BeanFactory`보다 다양하며, `ApplicationContext`를 주로 사용한다고 한다.

각각의 기능을 간단하게 살펴보면, 다음과 같다
- 국제화를 지원하여 텍스트 메시지를 관리해줌
- 로컬, 개발, 운영등을 구분해서 처리
- 리스너로 등록된 빈을 구독하여 이벤트 발생을 알 수 있음
- 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회


### ApplicationContext 설정
Spring Container인 ApplicationContext도 도식을 보면 여러 구현체들이 존재함을 알 수 있다. 예제에서 사용했던 `AnnotationConfigApplicationContext`도 (이름 참 기네) `ApplicationContext`의 구현체임을 알 수 있다. 이렇게 여러 구현체들이 존재하여 Spring 컨테이너는 다양한 형식의 설정 정보를 받아드릴 수 있도록 설계되어 있다.

개발자가 설정한 설정 정보인 `AppConfig.class`를 `AnnotationConfigApplicationContext`를 통해 넘기면 되는 것처럼 말이다. 

Java 코드를 기반하지 않아도 XML로 Bean을 등록하는 경우에, 이 설정 정보를 설정하기 위해선 `GenericXmlApplication`을 사용한다. 

`appConfig.xml` 에 빈 정보를 등록해 두었으면, 아래 처럼 등록할 수 있다.
```java
ApplicationContext ac = new
  GenericXmlApplicationContext("appConfig.xml");
```


이 외에도 `...ApplicationContext`형식의 여러 구현체를 활용하여 설정 정보를 유연하게 받을 수 있다.

### BeanDefinition
Spring Container가 여러 형식에 유연할 수 있는 이유는, 결국 등록되는 Bean에 대한 추상화(BeanDefinition)에 의존하고 있기 때문이다.
![](https://velog.velcdn.com/images/roycewon/post/5ce93583-ac36-4379-8c46-28274ae0dae2/image.png)

ApplicationContext를 구현한 어떤 구현체들 모두, 설정 정보를 읽어 `BeanDefinition`을 생성하기 때문에 가능하다는 것이다.

이렇게 생성된 `BeanDefinition`은 다음과 같은 정보를 가진다.
- BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
- factoryBeanName: 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig 
- factoryMethodName: 빈을 생성할 팩토리 메서드 지정, 예) memberService
- **Scope: 싱글톤(기본값)**
- lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한 생성을 지연처리 하는지 여부
- InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- DestroyMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)

### 정리
- Spring Container는 `BeanFactory`, `ApplicationContext`임.
이 때, `ApplicationContext`는 (`BeanFactory` + `여러 인터페이스`)를 상속 받아 더 많은 기능을 제공해준다.

- Spring Container의 큰 역할 중 하나는 Bean을 관리하는 것. 이러한 Bean을 유연하게 등록 가능 하도록 `BeanDefinition`이라는 추상화를 사용한다.


🔎 더 알아볼 내용

IoC Container가 Bean을 Singleton으로 관리한다!
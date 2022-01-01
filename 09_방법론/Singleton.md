# Singleton

싱글톤 패턴은 자바에서 많이 사용되는 디자인 패턴으로,

**"어떤 클래스가 최초 한번만 메모리를 할당하고 그 메모리에 객체를 만들어 사용한다"** 는 의미를 가지고 있다.

-> 생성자의 호출이 반복적으로 이루어 져도 실제로 생성되는 객체는 최초에 생성된 객체만 반환된다는 것

더 간단히, 클래스의 인스턴스인 객체는 최초에 딱 한번만 생성되도록 하는 패턴.



### 작성법

```java
public class SingletonClass {
  // 1
  private static SingletonClass singleton = new SingletonClass();
  
  // 2
  private SingletonClass() {}
  
  // 3
  public static SingletonClass getSingletonClassInstance() {
    return singleton;
  }
}
```

`1` : static 키워드를 통해 메모리에 인스턴스를 할당한다 

`2` : `private` 생성자를 통해 외부에서 `SingletonClass` 를 더이상 생성하지 못하도록 한다.

`3` : 생성된 singleton 객체를 반환해준다.

위와 같은 코드는 아주 작은 규모에서 사용할 수 있다.
`SingletonClass` 가 사용되지 않더라도 메모리에 할당되기 때문에 좀 더 개선된 작성법을 살펴보도록 하자



```java
public class SingletonClass {
  private static SingletonClass singleton;
  
  private SingletonClass() {}  
 	// 1
  public static SingletonClass getSingletonClassInstance() {
    if(singleton == null){
      singleton = new SingletonClass();
    }
    return singleton;
}
```

`1` : 메서드를 변경하여 해당 객체가 사용되는 경우에 할당되도록 변경하였다.

```java
import static org.assertj.core.api.Assertions.*;

import org.junit.jupiter.api.Test;

public class PlayGround {
	@Test
	void test() {
		SingletonClass a = SingletonClass.getSingletonClassInstance();
		SingletonClass b = SingletonClass.getSingletonClassInstance();
		SingletonClass c = SingletonClass.getSingletonClassInstance();
		
    assertThat(a).isEqualTo(b).isEqualTo(c);
	}
}
//성공
```





### 사용 이유

- 싱글톤 패턴을 사용하는 이유는 재사용을 통한 메모리 낭비를 방지할 수 있다.

  >한번의 new연산자를 통해서 고정된 메모리 영역만 사용하기 때문에 메모리 낭비를 줄일 수 있을 뿐만아니라, 이미 생성된 인스턴스를 활용함으로써 속도 측면에서의 유리함도 챙길 수 있다.

  

- 싱글톤으로 생성된 객체는 유일성이 보장되기 때문에 공유하여 사용되기 용이하다.

  > 싱글톤 인스턴스가 전역으로 사용될 수 있기 때문에 다른 클래스가 접근하여 사용할 수 있다.

- 인스턴스가 한 개만 존재한다는 것을 보증할 수 있다.

  > 도메인 관점에 적합하게 구현할 수 있다.



### 문제점

싱글톤 패턴은 문제점들이 비교적 많다. 그래서 사용하기전, 이점과 문제점을 비교하여 trade-off를 고려하여야 한다.

- 다량의 코드 필요성

  >싱글톤 패턴을 구현하기 위해선 위와 같은 싱글톤을 보장하기 위해 여러 코드를 작성했다. 위 예시에서의 코드는 단순 생성에서의 구현일뿐, 만약 규모가 큰 프로그램이라면 멀티스레딩 환경에 대한 동시성 문제를 해결해야 한다. 공유하는 인스턴스인 만큼, 동시성의 문제가 필히 발생한다. 이를 방지하기 위해 `syncronized` 키워드를 활용하여 추가적인 코드를 작성하여야 한다.

  

- 테스트에서의 문제

  >싱글톤 인스턴스는 자원을 공유하고 있기 때문에 테스트가 격리된 환경에서 수행되려면 매번 인스턴스의 상태를 초기화 시켜주어야 한다. 그렇지 않으면 전역에서 공유된 상태가 존재하기 때문에 독립적인 테스트를 온전히 수행할 수 없다. 그렇다고 테스트를 위해 인스턴스의 상태를 초기화 한다면, 다른 메인 로직에서 사용하고 있는 경우에 문제가 발생할 수 있다.

  

- 객체지향원칙 위배

  > 앞서 코드에서 살펴본 것처럼 `PlayGround` 에서 `SingletonClass.getSingletonClassInstance();` 를 통해 구체 클래스에 대해 의존하게 된다. 이렇게 상위 모듈이 하위 모듈에 종속되어 있기에 SOLID중 DIP를 위반하게 되고, OCP도 위반할 가능성이 높아진다.

- 자식클래스 생성 불가



### 정리

싱글톤 패턴을 통해 한 개의 인스턴스 생성을 보증한다면 메모리와 속도 측면에서 효율적인 부분을 기대할 수 있지만 수반되는 문제점도 많이 있다는 것을 알 수 있다. 

특히, 수반되는 문제점들 중에선 객체 지향에 위반되는 사례가 있다.

이러한 이점을 살리면서 객체 지향을 지키기 위해 스프링(Spring) 프레임워크에서는 컨테이너를 제공하고 있다. Spring을 사용하면서 Bean을 생성했다면, 싱글톤임을 인지하고 있어야 할 것이다.
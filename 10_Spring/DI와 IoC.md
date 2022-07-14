# DI와 IoC

## 들어가며

Spring을 처음 공부하거나 접하는 경우, 항상 언급되는 용어(개념)이 있다. 바로 **DI**와 **IoC**이다. Spring Framework를 사용하거나 공부하는 사람이라면, 반드시 이해하고 있어야 한다고 한다. 그런 사람으로서 DI와 IoC에 대해서 알아보자.

 

## IoC

**I**nversion **o**f **C**ontrol : 제어의 역전

아는 단어로들로 이루어져 있지만, **어떤 제어**가 **어떤 관계에서의 역전**이 이루어지는 지는 걸까?

다음과 같은 예시를 살펴보자

객체지향나라에서는 여러 카드사들이 존재한다. 우리는 카드사마다 다른 수수료를 반영하면서 결제가 이루어지는 시스템을 만들어야 한다.

![img](https://velog.velcdn.com/images/roycewon/post/8473e752-61e5-4cab-ab0e-71767b2adde5/image.png)

단순히 위 구조를 사용하면, `Interface`인` CardCompany`를 활용하여 여러 카드사의 수수료를 달리 반영하며 결제 서비스를 만들 수 있다. (OCP 원칙)



`CardCompany.class`

```java
interface CardCompany {
  int getPaymentFee();
}
```

```java
public class CompanyA implements CardCompany {
	private int PAYMENT_FEE = 3;
  
  public int getPaymentFee() {
    return PAYMENT_FEE;
  };
}

public class CompanyB implements CardCompany {
	private final int PAYMENT_FEE = 6;
  
  public int getPaymentFee() {
    return PAYMENT_FEE;
  };
}
```



`PayService.class`

이제 일반적인(?) 개발자라면 다음과 같은 순서로 개발할 것이다.

1. 객체 생성
2. 의존성 객체 생성
3. 객체 메서드 호출

```java
public class PayService {
  
  public void pay() {
    CardCompany cardCompany = new CompanyA(); //객체 생성 및 의존성 객체 생성
    cardCompany.getPaymentFee(); //객체 메서드 호출
  }
}
```

만약 위 상황에서, `CompanyB`의 결제방식을 따라야 한다면, 개발자가 직접 부여한 의존성을 변경해야 한다.

기껏 객체지향을 준수하면서 설계해놓고, DIP, OCP를 위반하게 되었다

![img](https://velog.velcdn.com/images/roycewon/post/5df9760a-ce9e-41c6-9ac7-4708ad654889/image.png)



이렇게 코드 작성자(개발자)가 주도적으로 객체를 생성하고, 의존 관계를 설정하고 사용하면서 제어권을 가진다면 객체지향원칙을 위반할 경우가 생긴다.

이런 제어권(객체 생성, 의존관계 설정)을 위임하면 어떨까?

`AppClassContainer.class`

```java
public class AppClassContainer {
  public CardCompany cardCompany() {
    return new CompanyA //CompanyA를 생성 후 반환
  }
}
```

이런 제어권을 가진 클래스가 존재하면, `PayService`는 다음과 같이 바꿀 수 있다.

```java
public class PayService {
  AppClassContainer appClassContainer = new AppClassContainer;
  
  public void pay() {
    CardCompany cardCompany = appClassContainer.cardCompany();
    cardCompany.getPaymentFee();
  }
}
```

`PayService`에서는 이제 구체적인 구현체 (CompanyA, CompanyB)에 대해서 전혀 모르고 있다. 다만, `CardCompany`의 구현체를 결정해주는 `AppClassContainer`만 알고 있다.

`PayService`는 이제 구체적인 객체를 생성하거나 의존관계를 생성하지 않는다. 그냥 `AppClassContainer`가 정해놓은 관계를 따르기만 할 뿐이다.

![img](https://velog.velcdn.com/images/roycewon/post/00aaf3ec-0bc0-4591-980c-f8e71f1ca997/image.png)

코드를 좀 더 개선하면 다음과 같이 구성할 수 있다.

```java
public class PayService {
  CardCompany cardCompany;
  
  public PayService(CardCompany cardCompany) {
    this.cardCompany = cardCompany;
  }
  
  public void pay() {
    cardCompany.getPaymentFee();
  }
}
```



`AppClassContainer`에서 이미 의존관계를 다 설정해 둔 `PayService`를 생성

```java
public class AppClassContainer {
  public PayService payService() {
    return new PayService(new CompanyA()); //CompanyA를 생성 후 반환
  }
}
```



#### 제어의 역전

`AppClassContainer`의 등장으로 `PayService`는 필요한 인터페이스를 모두 호출했지만 어떤 구현 객체들이 실행되는지 모른다. 객체를 생성하고 의존관계를 설정하는 제어권이 없어졌기 때문이다.

이렇게 프로그램의 제어 흐름을 직접 제어하기 않고 외부에 위임하여 관리되도록 하는 것을 제저의 역전(**IoC**)라고 한다.

첫 질문 이었던 "**어떤 제어**가 **어떤 관계에서의 역전**이 이루어지는 지는 걸까?"의 대답으로
**객체를 생성하고, 연결하고, 실행하는 등 프로그램의 제어**를가
로직을 직접 실행하여 프로그램을 동작하는 구현 객체가 아닌 **다른 객체(외부)**에서 이루어진다고 대답할 수 있다.

`AppClassContainer`처럼 Spring에서는 `Spring Container`가 IoC를 담당한다(제어권을 가진다).



### DI

제어권을 가진 Spring Container는 위 예시처럼 의존관계를 설정해주는 역할도 가진다.

구현 객체에서 의존관계를 설정하는 것이 아닌, 주입 받는 것을 **DI**(**D**ependency **I**njection)라고 한다.

```java
public class PayService {
  private CardCompany cardCompany;
  public void pay() {
    //구현 객체에서 직접 생성
    cardCompany = new CompanyA(); 
    cardCompany.getPaymentFee();
  }
}
```

```java
public class PayService {
  //의존 관계를 직접 설정X
  private CardCompany cardCompany;
  
  public PayService(CardCompany cardCompany) {
    this.cardComapny = cardCompany;
  }
  
  public void pay() {
    cardCompany.getPaymentFee();
  }
}

//DI
//AppClassContainer에서 PayService의 의존 관계를 설정함
public class AppClassContainer {
  public PayService payService() {
    return new PayService(new CompanyA()); //CompanyA를 생성 후 반환
  }
}
```



> IoC Container는 모든 객체를 관리하지 않는다. 빈(Bean)으로 등록된 객체들을 어플리케이션이 실행될때 모두 생성하고 의존관계가 설정된다. 자세한 정보는 Bean-LifeCycle을 참고하자



### DI 구현 방식

1. 필드 주입

   ```java
   @Bean
   public class PayService {
     @Autowired
     private CardCompany cardCompany;
   }
   ```

   사용법 : 변수 선언부에 `@Autowired` 어노테이션을 붙힌다.

   > 이 방법은 사용법이 매우 간단하다. 하지만 의존 관계 파악이 어렵다는 점이 있다. 또, 순환 참조가 발생 할 수 있다.

2. 수정자 주입

   ```java
   @Bean
   public class PayService {
   
     private CardCompany cardCompany;
     
       @Autowired
     public void setCardCompany(CardCompany cardCompany) {
       this.CardCompany = cardCompany;
     }
   }
   ```

   Setter를 사용하여 의존관계 주입

   > 의존성을 선택적으로 주입할 수 있다. 하지만 위험하다. Setter가 호출되지 않아 cardCompany에 아무런 의존관계가 주입되지 않아도 실행이 가능하기 떄문이다.

3. 생성자 주입 (권장)

   ```java
   public class PayService {
     private CardCompany cardCompany;
   
     @Autowired
     public PayService(CardCompany cardCompany) {
       this.cardComapny = cardCompany;
     }
   }
   ```

   생성자에 `@Autowired` 어노테이션을 붙힌다.

   > 의존관계가 주입되면서 생성되기 때문에 수정자 주입에서 발생할 수 있는  NullPointerException을 방지할 수 있다.
   >
   > 불변성을 보장할 수 있다.
   >
   > 순환참조가 컴파일 단계에서 드러난다.
   >
   > 만약, 생성자 인자가 많아지면 코드가 길어질 수 있다.



### IoC와 DI를 써야할까?

이제 IoC와 DI에 대한 개념을 이해하였다. 그럼 이러한 패턴을 왜 사용해야 하는 걸까?

객체지향 프로그래밍의 장점(유지 보수, 확장성 등)을 얻으려면 SOLID 원칙을 준수해야한다. 이를 준수하기 위해 고안된 패턴이라고 생각하면 쉽게 떠올릴 수 있을 것이다.

우선, IoC. Spring Application의 객체(Bean)을 개발자는 직접 관리할 필요가 없어진다. IoC 컨테이너에서 결정해주고 관리해주기 때문에 비즈니스 로직에만 집중할 수 있다는 장점이 있다.

DI의 장점은, 의존 정도가 줄어든다는 것이다. 의존 관계의 유무는 동일하게 존재하지만, 그 의존도가 낮아진다. 외부에서 주입받기 때문에 다른 객체를 의존하기 위해 교체해야 하는 경우, 주입해주는 Container에서만 변경하면 되기 때문이다.

이를 활용하여, 재사용성도 높아진다. 하나의 구현 코드에 대해서 의존관계를 달리 주입하면 모두 다른 구체 동작이 이루어지지 않는가?



### 정리

Spring을 사용하면 꼭 알고 있어야 하는 개념이라 생각이 든다. 추상적으로라도 Spring의 전체적인 구조나 흐름에 대해 이해하고 사용하는 것은 매우 큰 도움이 된다.



### 🔎 더 알아볼 내용 

- `IoC Container`의 추가적인 역할과 `BeanFactory`

- `@Autowired`의 동작 방식

- 순환참조



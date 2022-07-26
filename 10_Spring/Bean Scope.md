# Bean Scope

기본적으로 Spring Container가 관리하는 Bean은 싱글톤으로 관리된다고 알고 있다. 기본값이 싱글톤인 것이지, 무조건 싱글톤은 아니다.

이에 관련하여, Bean의 Scope(빈의 존재 범위)에 관해 어떤 설정들을 제공하는 지 알아보자.

## @Scope

`@Bean`, `@Component` 대상에 추가적으로 붙히면 된다. `@Scope`의 기본값은 `"singleton"` 이다.

```java
@Scope
@Component
static class SingletonBean {
}

@Test
void singletonBeanFind() {
	AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);

	SingletonBean bean1 = ac.getBean(SingletonBean.class);
	SingletonBean bean2 = ac.getBean(SingletonBean.class);

	System.out.println("bean1 = " + bean1);
	System.out.println("bean2 = " + bean2);

	assertThat(bean1).isSameAs(bean2);

}
```

![img](https://velog.velcdn.com/images/roycewon/post/d9f804fe-1bb8-4a95-b672-57a2a9c402e1/image.png)

이미 알고 있듯이 테스트 결과는 `@Scope`의 존재와 관계없이 성공일 것이다.

### @Scope(value = "prototype")

```java
@Scope(value = "prototype")
@Component
static class SingletonBean {
}

@Test
void singletonBeanFind() {
	AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);

	SingletonBean bean1 = ac.getBean(SingletonBean.class);
	SingletonBean bean2 = ac.getBean(SingletonBean.class);

	System.out.println("bean1 = " + bean1);
	System.out.println("bean2 = " + bean2);

	assertThat(bean1).isSameAs(bean2);

}
```

![img](https://velog.velcdn.com/images/roycewon/post/8643de89-c980-42db-a097-cc424fd78609/image.png)

`@Scope(value = "prototype")` 만으로도 동일했던 테스트가 실패했다. `SingetonBean`이라는 객체는 더이상 싱글톤으로 관리되지 않도록 설정한 것이기 때문이다. (각각 @4426bff1, @3c7c886c 라는 다른 값을 가짐)

여기서 재밌는 점은 Spring Container는 싱글톤으로 관리할 필요가 없는 Scope가 Prototype인 객체를 주입, 초기화 단계만 거치고 관리하지 않는다는 것이다.

위 테스트 코드에서 `@PreDestory`를 통해 종료메서드를 등록하고 컨테이너를 종료해도 해당 메서드가 실행 되지 않는 것을 알 수 있다.

```java
@Scope(value = "prototype")
@Component
static class SingletonBean {
	@PreDestroy
	public void destroy() { //종료 메서드
		System.out.println("finish");
	}
}

@Test
void singletonBeanFind() {
	AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);

	SingletonBean bean1 = ac.getBean(SingletonBean.class);
	SingletonBean bean2 = ac.getBean(SingletonBean.class);

	System.out.println("bean1 = " + bean1);
	System.out.println("bean2 = " + bean2);

	assertThat(bean1).isNotSameAs(bean2);
    ac.close();
    }
}
```

![img](https://velog.velcdn.com/images/roycewon/post/80251b4e-510a-4a73-8355-0313525581d2/image.png)

> destroy()는 실행되지 않았다

### Prototype과 싱글톤

Prototype은 객체를 생성한 뒤, 의존 관계를 주입하고 초기화해서 클라이언트에게 위임한다. 종료와 소멸에는 관여하지 않는 다는 말이다.
만약 싱글톤 빈에서 Scope가 Prototype인 빈에 의존하고 있다고 하면, Prototype인 빈이더라도, 싱글톤인 객체가 계속 존재하고 있기 때문에 사용할 때마다 새로 생성되지 않을 것이다.


아래 코드로 살펴보자
Scope가 싱글톤인 `SingletonBean`과 prototype인 `PrototypeBean`이 있다고 하자.

```java
@Scope("prototype")
@Component
static class ProtoTypeBean {
	private int stacking = 0;
    
    public void addStacking() {
    	stacking++;
    }
    
    public int getStacking() {
    	return stacking;
    }
}

@Component
static class SingletonBean {
	ProtoTypeBean protoTypeBean;

	public SingletonBean(ProtoTypeBean protoTypeBean) {
		this.protoTypeBean = protoTypeBean;
	}

	public void logic() {
		protoTypeBean.addStacking();
	}
}
```

이때, `SingletonBean`은 `PrototypeBean`에 의존하고 있다.

Test를 해보자.

```java
@Test
void singletonBeanFindTest() {
	AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class, ProtoTypeBean.class);

	SingletonBean bean1 = ac.getBean(SingletonBean.class);
	bean1.logic();
	assertThat(bean1.protoTypeBean.getStacking()).isEqualTo(1);

	SingletonBean bean2 = ac.getBean(SingletonBean.class);
	bean2.logic();
	assertThat(bean2.protoTypeBean.getStacking()).isEqualTo(1);
}
```

![img](https://velog.velcdn.com/images/roycewon/post/c18db835-be91-4f2d-86e8-9ef80202f011/image.png)

bean1, bean2로 `SingletonBean`을 두번 사용했지만, SingletonBean이 의존중인 `ProtypeBean`도 싱글톤 처럼 동일하게 유지되어 `Stacking()`이 1이 아닌 2임을 알 수 있다.
Prototype은 빈의 소멸과 종료를 책임지지 않기 때문이다. Scope가 Singleton인 `SingletonBean`(클라이언트)에 종속적이게 되었다.

### Provider

만약, 싱글톤으로 관리되는 빈에 Prototype의 특성(사용시 마다 생성)을 가진 빈을 의존하게 하고 싶다면, 다음과 같은 선택사항이 있다.

- ObjectProvider (From Spring)
- Provider (javax)

아까와 동일한 코드에 위 방식을 적용해보고 테스트를 하면 의도했던대로 동작한다!

**ObjectProvider**

```java
@Component
static class SingletonBean {
	ObjectProvider<ProtoTypeBean> protoTypeBeanProvider; //ObjectProvider 추가

	public SingletonBean(ObjectProvider<ProtoTypeBean>  protoTypeBeanProvider) {
		this.protoTypeBeanProvider = protoTypeBeanProvider;
	}

	public int logic() {
    	PrototypeBean prototypeBean = protoTypeBeanProvider.getObject();
        System.out.println("SingletonBean depends on : " + protoTypeBean);

		protoTypeBean.addStacking();
        return protoTypeBean.getStacking();
	}
}

@Test
void singletonBeanProtoTest() {
	AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class, ProtoTypeBean.class);

	SingletonBean bean1 = ac.getBean(SingletonBean.class);
	int logic1 = bean1.logic();
    System.out.println(bean1.protoTypeBeanObjectProvider.getObject());
	assertThat(logic1).isEqualTo(1);

	SingletonBean bean2 = ac.getBean(SingletonBean.class);
    System.out.println(bean1.protoTypeBeanObjectProvider.getObject());
	int logic2 = bean2.logic();
	assertThat(logic2).isEqualTo(1);
}
```

![img](https://velog.velcdn.com/images/roycewon/post/30f6d55b-4ad8-49a3-a2db-6e32d0fd2b1d/image.png)
ObjectProvider에서 getObject()를 할 때 마다 새로운 객체가 생성되서 반환된다는 것도 알 수 있다.

다른 방법으로는 `javax.inject:javax.inject:1`라는 라이브러리를 추가하면 사용할 수 있는 `Provider`방식이 존재한다.

위 방식과 동일하게 동작하니 간단하게 적용만 살펴보자

```java
@Component
static class SingletonBean {
	Provider<ProtoTypeBean> protoTypeBeanProvider; //ObjectProvider 추가

	public SingletonBean(Provider<ProtoTypeBean>  protoTypeBeanProvider) {
		this.protoTypeBeanProvider = protoTypeBeanProvider;
	}

	public int logic() {
    	PrototypeBean prototypeBean = protoTypeBeanProvider.get();
        System.out.println("SingletonBean depends on : " + protoTypeBean);

		protoTypeBean.addStacking();
        return protoTypeBean.getStacking();
	}
}
```

실제로 이런 케이스가 존재하는지에 대해서는 의문이 든다. Singleton으로 관리되어야 하는 객체 안에, 공유 되서는 안되는 객체를 의존한다는 건 설계의 오류가 아닐까 하는 의문이 든다.

어떤 객체를 사용할 때 마다 매번 새로운 동작(?), 작동을 해야하는 거라면, 객체를 사용하였던 시간을 측정하거나 (0부터 ~ 종료까지), 객체의 사용자를 체크하는데 사용할 수 있을 것 같다.

### @Scope("request")

Request는 하나의 HTTP 요청에 대해 각각 생성되는 범위이다. Prototype과는 다르게 종료시점까지 Spring Container에서 관리해준다.

간단한 웹 프로젝트를 만들어 보자.
매 요청마다 걸린 시간을 기록하는 역할을 하는 객체가 있다고 해보자.

```java
@Component
@Scope(value = "request")
public class WebTimeTracker {
	private String uuid;
	private Long startTime = System.currentTimeMillis();

	public String getUuid() {
		return uuid;
	}

	@PostConstruct
	public void init() { //생성시 UUID와 시작시간 설정
		uuid = UUID.randomUUID().toString();
		System.out.println("[" + uuid + "]" + " : Init at " + startTime);
	}

	@PreDestroy
	public void destroy() { //종료시 걸린시간 출력
		long endTime = System.currentTimeMillis();
		System.out.println("[" + uuid + "]" + " : End at " + endTime);
		System.out.println("[" + uuid + "]" + " : It takes " + (endTime - startTime));
	}
}
@Controller
public class WebTimeTrackerController {
	private final ObjectProvider<WebTimeTracker> webTimeTrackerProvider;

	public WebTimeTrackerController(ObjectProvider<WebTimeTracker> webTimeTracker) {
		this.webTimeTrackerProvider = webTimeTracker;
	}

	@RequestMapping("time-check")
	@ResponseBody
	public String timeCheck() {
		WebTimeTracker webTimeTracker = webTimeTrackerProvider.getObject();
		System.out.println(webTimeTracker.getUuid() + " = " + webTimeTracker.getClass());
		return "OK";
	}
}
```

간단하게 정리하면, HTTP 요청이 들어오면 요청이 들어온 시간과 종료되는 시간을 측정해서 걸린 시간을 출력하는 역할을 한다.

> 이때, 주의해야하는 점이 있는데, `Scope("request")`는 HTTP요청이 들어오는 경우에만 생성이 된다. 따라서, Controller에서 `Scope("request")`인 WebTimeTracker를 의존할 때는 `ObjectProvider`를 사용해야 한다.

실제 로그를 확인해보면 요청마다 새로운 `WebTimeTracker`가 생성되고 소멸하는 것을 알 수 있다.

![img](https://velog.velcdn.com/images/roycewon/post/20dc3f76-b520-47fe-bcb3-cd0e21a8d968/image.png)
요청이 너무 빨라 일괄적으로 보이니.. Thread를 잠시 지연시켜 섞이도록 해보겠다.

```java
@RequestMapping("time-check")
@ResponseBody
public String timeCheck() {
	WebTimeTracker webTimeTracker = webTimeTrackerProvider.getObject();
    Thread.sleep(1000); //1초 지연
	System.out.println(webTimeTracker.getUuid() + " = " + webTimeTracker.getClass());
	return "OK";
}
```

![img](https://velog.velcdn.com/images/roycewon/post/3b590f8a-5092-4166-ab08-b0940ac08c04/image.png)
위 로그를 확인해보면 싱글톤으로 공유되는 것이 아닌 각각의 UUID를 가진 새로운 객체들이 생성된다는 것을 알 수 있다!!

### cf) 프록시

`@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)` 라는 Scope 조건을 달면, ObjectProvider를 사용하여 HTTP요청을 대기하는 코드를 줄일 수 있다고 한다.

```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class WebTimeTracker {
	private String uuid;
	private Long startTime = System.currentTimeMillis();

	public String getUuid() {
		return uuid;
	}

	@PostConstruct
	public void init() {
		uuid = UUID.randomUUID().toString();
		System.out.println("[" + uuid + "]" + " : Init at " + startTime);
	}

	@PreDestroy
	public void destroy() {
		long endTime = System.currentTimeMillis();
		System.out.println("[" + uuid + "]" + " : End at " + endTime);
		System.out.println("[" + uuid + "]" + " : It takes " + (endTime - startTime));
	}
}
@Controller
public class WebTimeTrackerController {
	private final WebTimeTracker webTimeTracker;

	public WebTimeTrackerController(WebTimeTracker webTimeTracker) {
		this.webTimeTracker = webTimeTracker;
	}

	@RequestMapping("time-check")
	@ResponseBody
	public String timeCheck() throws InterruptedException {
		Thread.sleep(1000);
		System.out.println(webTimeTracker.getUuid() + " = " + webTimeTracker.getClass());
		return "OK";
	}
}
```

![img](https://velog.velcdn.com/images/roycewon/post/0e179564-fa62-41fd-957e-7b052134f942/image.png)
여기 로그에서 재밌는 점이 보인다. `xxxCGLB$$aba96306`이 보이는가? 분명 새로운 UUID가 생성되는 걸 보면 객체도 새로 생성되는 것처럼 보이지만, 실제론 같은 객체(`$$aba96306`)가 호출되고 있다. `@Configuration`에서의 `xxxCGLB` 처럼 바이트코드를 조작해 우리가 설정한 `WebTimeTracker`를 참조하여 싱글톤으로 관리하는 다른 객체가 생성되었음을 알 수 있다.
`ObjectProvider`를 사용하지 않아 편리하긴 하지만, 의도와는 다르게 Singleton객체를 통해 "request"처럼 관리한다는 점에 주의하면 좋을 것 같다
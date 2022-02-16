# @ComponentScan

### ComponentScan이란?

**설정된 지점을 포함한 하위 패키지**들을 scanning하며 `@Component`어노테이션이 붙은 클래스들을 빈으로 등록해주는 역할을 수행한다.

>`@Repository`, `@Controller`, `@Service`와 같은 어노테이션들은 `@Component`를 포함하고 있기 때문에 `@ComponentScan`의 대상이 된다.



## ComponentScan 설정하기

`@SpringBootApplication`을 살펴보면 다음과 같은 `@ComponentScan`이 존재한다.

```java
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
}
```

우선, `excludeFilters`를 제외하면 다른 설정은 존재하지 않는다. `@ComponentScan`은 `basePackageClasses()`나 `basePackage()`를 통해 scanning 범위를 변경할 수 있다.

예제 Test

### ComponentScanRange.class 

```java
package level1;

@Component
public class ComponentScanRange {
  public int number = 10; //예시를 위해 public 사용
}
```

### Application.class

```java
package level1.level2;

@SpringBootApplication
@Controller
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(PlaygroundApplication.class, args);
	}
}
```

`@SpringBootApplication`가 포함된 `Application`클래스보다 더 상위 패키지에 `ComponentScanRange`클래스가 존재하는 구조이다.

### 조회 Test

```java
@ExtendWith(SpringExtension.class)
@SpringBootTest
class SpringTest {
	@Autowired
	private ComponentScanRange componentScanRange;

	@Test
	public void Test() {
		assertThat(componentScanRange.number).isEqualTo(10);
	}
}
```

Scan범위 밖의 `@Component`에 대해서 `@Autowired`를 통해 주입 받지도 못하고 컴파일 에러가 난다.

### Scan범위 변경

```java
package level1.level2;

@SpringBootApplication
@Controller
@ComponentScan(basePackageClasses = ComponentScanRange.class)
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(PlaygroundApplication.class, args);
	}
}
```

범위를 변경 한 뒤, Test를 수행하면 잘 작동한다!

이름에서도 알 수 있듯이 `~class`, `~Package`가 아닌 `-s`가 붙어서 끝난다.

즉, `basePackages`나 `basePackageClasses`를 여러개 설정 할 수 있다.

> Spring Docs에서는 다음과 같이 `basePackageClasses`와 `basePackages`에 대해 정의하고 있다.
>
> `basePackageClasses()`가 Type-safe임을 알수 있다
>
> | Modifier and Type | Optional Element and Description                             |
> | :---------------- | :----------------------------------------------------------- |
> | `Class<?>[]`      | `basePackageClasses` : Type-safe alternative to `basePackages()`for specifying the packages to scan for annotated components. |
> | `String[]`        | `basePackages` : Base packages to scan for annotated components. |

### Filter

추가로 다음과 같은 Optional Element들이 있다.

| Modifier and Type        | Optional Element and Description                             |
| :----------------------- | :----------------------------------------------------------- |
| `ComponentScan.Filter[]` | `excludeFilters`Specifies which types are not eligible for component scanning. |
| `ComponentScan.Filter[]` | `includeFilters`Specifies which types are eligible for component scanning. |

짐작해 보면 `Filter`를 통해 Scan할 Component들을 직접 추가하거나 제외함으로써 세부적인 설정이 가능할 것 같다.

`@SpringBootApplication`내부의 `@ComponentScan`을 살펴보자.

```java
@ComponentScan(
  excludeFilters = {
    @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) 
  }
)
```

`@Filter`어노테이션에 대해 자세히 모르지만, `TypeExcludeFilter`클래스와 `AutoConfigurationExcludeFilter`클래스를 제외하고 Scan함을 알 수 있다. `@Filter`에 대한 자세한 내용은 추후 다루도록 하자.

중요한건, `@ComponentScan`을 조작하여 Scan할 `@Component`에 대해 제어할 수 있다는 것이다.

추가로, `useDefaultFilters=false`를 통해 기본적으로 Scan하는 `@Component`, `@Controller`, `@Service`, `@Repository`를 Scan하지 않는다.



## Xml설정으로 등록

`.xml`파일에서 `ComponentScan`에 대한 설정을 할 수 있다.

`basePackages()`

```xml
<context:component-scan base-package="level1"/>
```

`excludeFilters`

```xml
<context:component-scan base-package="com.zorba.redWine">
	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
</context:component-scan>
```





`@ComponentScan`은 프로그램이 실행 될때, 싱글톤 Scope인 빈들을 모두 생성한다. 사용되지 않아도 `@Bean`으로 등록될 클래스들도 모두 등록하기 때문에 초기 구동시간이 오래 걸릴 수 있다는 단점이 있을 수 있기 때문에, `exclude`를 활용하여 단점을 채울 수 있지 않을까 하는 생각이 든다.



### 🔎 더 알아볼 내용 

- `@ComponentScan`의 동작 과정

- `@Filter`

- `IoC Cotainer`와 `DI`

  

### ❓의문점

- 초기에 모든 `@Bean`을 등록하지 말고 상황에 맞게 사용할 수 있을지 의문.


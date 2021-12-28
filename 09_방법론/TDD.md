# TDD

### (Test Driven Development)

### TDD란

"테스트 주도 개발" 이라는 의미로, 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 구현하는 단계를 반복적으로 수행하며 소프트웨어(프로그래밍)을 구현한다는 방법론.

보통 프로그램을 개발하겠다고 생각한다면, 설계 -> 개발 -> 테스트과 같은 단계를 거칠 것이다. 사실 개발 뿐만아니라 문제를 해결하기 위해선 이런 사고과정이 지배적인것 같다. 제품도 만들어져야 테스트를 할 수 있듯이, 코드도 구현이 되어있어야 테스트를 하지 않겠는가.

하지만 TDD는 테스트를 먼저 만들어 내고, 이를 구현하고, 리팩토링하는 짧은 개발주기를 반복하며 Extream Programming을 실현할 수 있다.

>*`Extream Programming` : 미래에 대한 예측을 줄이고, 지속적으로 프로토타입을 완성하는 애자일 방법론 중 하나.*



- 예시

우선, 예시로 살펴보자

```java
class Person {
	String name;
	int age;

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}
```

`Person`이라는 객체가 존재한다고 했을때, 두 가지 기능사항을 요구받았다고 해보자.

- 이름을 통해 사람을 찾는 기능
- 나이가 20살 이상이면 성인

일반적인 방식이라면, 메서드를 생성 -> 기능 구현 -> 테스트 라는 순서로 개발할 것이다.

TDD에서는 테스트 부터 설계한다.

`이름을 통해 나이를 찾는 기능`에 대한 테스트

1. 존재하는 이름인지에 대한 테스트가 필요함
2. 없으면 False, 있으면 True를 반환하여야 한다

```java
public class funcTest() {
  @Test
  void isValidNameTest() {
    //given
    PersonRepo ps = new PersonRepo();
    
    //when
    Person person = new Person("Royce", 15);
    ps.addPerson(person);
      
    //then
    Assertions.assertThat(ps.isValidName("Hello"))
      .isFalse();
  }
}
```

이렇게 테스트만 먼저 설계해둔다.

`PersonRepo` 클래스나 `findByName` 메서드도 존재하지 않은채 이렇게 테스트를 하겠다라고 테스트를 만들어 두고,

해당 로직을 만든다.

```java
public class PersonRepo {
  List<Person> personRepository = new ArrayList<>();
  
  public void addPerson(Person person) {
    personRepository.add(person);
  }
  
  public boolean isValidName(String name) {
    return personRepository.stream()
      .filter(p -> p.getName().equals(name))
			.findAny()
			.isPresent();
  }
}
```

 만든 뒤 설계해 두었던 Test를 실행하여 통과하면 새로운 테스트를 추가하고
(존재하는 이름을 통해 Person조회하기) 

통과하지 못하면 로직으로 돌아가 수정하여 완성한다.

이러한 단위테스트들의 반복을 수행하며 프로그래밍을 해 나아가는 방식이다.


### Why TDD?

TDD는 간단하게 생각하면 일반적인 개발 순서만 뒤집힌 것 뿐이라고도 생각할 수 있다. 그 이상의 기대값에 대해 무게를 좀 더 실어보자.

1. 기존 방식에서의 문제점

   방법론의 존재 이유와 핵심적인 기대사항에 걸맞게 유지보수의 용이함과 재사용성을 높인다.

   기존 개발방식은 다음과 같은 문제에 치명적이다.

> 의뢰자의 요구사항이 초기단계와 항상 동일 할 수 없기 때문에 설계는 항상 변한다.

​	부분적인 변경사항에 대해서도 테스트를 위해 모든 프로그램 과정(로직)을 테스트해야하며 	이로 인한 버그 발생시, 어느 부분이 버그를 유발하는지 명확하게 찾아내기 어렵다.

2. 결정과 피드백 사이의 갭

   TDD를 공부하며 처음 알게된 개념이다.

   >`결정(decision)` : 개발 과정에서 '이 방법으로 해야지', '이걸 사용해서 해결해야지'와 같은 결정
   >
   >`피드백(feedback)` : 프로그램의 성공/실패라는 결과 

   이 둘(결정 - 피드백) 사이의 갭을 인지할 수 있다는 것이 장점이다.

   "이 방법으로 해야지"`결정` 의 "실패"  `피드백` 의 간격을 인식하는 것이 프로그램을 의도한 대로 작동하는데 중요하다고 한다.

   내가 이해한 뉘앙스는 문제 해결과 내 결정의 거리라고 이해하고 있다.

   

   
# HashCode

```java
public class Object{
	public native int hashCode();
}
//hashCode()의 반환값은 int형이다.
```

> *`native` : OS가 가지고 있는 메서드를 사용한다는 뜻*
>
> 

- 객체의 해시코드를 반환하는 메서드. 객체마다 hashCode는 모두 다르다.

- 객체의 인스턴스 변수를 통해 비교를 하고자 `equals()` 를 Override했다면, `hashcode()`도 Override 해야한다. `hashCode()` 를 통해 비교하기 때문이다.

>`System.identityHashCode()` : Override되기 이전의 HashCode를 반환해줌

- 예제

```java
class Person {
	String name;
	int age;
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}
public void play() {
  Person Royce1 = new Person("Royce", 23);
  Person Royce2 = new Person("Royce", 23);

  System.out.println(Royce1.hashCode()); //197964393
  System.out.println(Royce2.hashCode()); //1620890840
  System.out.println(Royce1.equals(Royce2)); //false
}
```

위 코드를 보면, `Royce1`과 `Royce2` 라는 같은 내용의 인스턴스 변수를 가지는 `Person`객체가 있다. 이 두 객체는 `hashCode()` 가 다르므로 `equals()`의 결과도 `false`라는 것을 확인할 수 있다.

- Override `equals()`

```java
@Override
public boolean equals(Object obj) {
  if (!(obj instanceof Person)) {
    return false;
  }
  Person person = (Person)obj;
  return this.age == person.age
    && this.name.equals(person.name);
}
```

`Object` 클래스로 부터 `eqauls()`를 Override를 하였다. `Person` 객체간의 비교만 가능하도록 하고, 인스턴스 변수인 age와 name이 동일하면 `true`를 반환하도록 구현하였다.

```java
class Person {
	String name;
	int age;
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
  @Override
  public boolean equals(Object obj) {
    if (!(obj instanceof Person)) {
      return false;
    }
    Person person = (Person)obj;
    return this.age == person.age
      && this.name.equals(person.name);
  }
}
public void play() {
  Person Royce1 = new Person("Royce", 23);
  Person Royce2 = new Person("Royce", 23);

  System.out.println(Royce1.hashCode()); //197964393
  System.out.println(Royce2.hashCode()); //1620890840
  System.out.println(Royce1.equals(Royce2)); //true
}

```

이렇게만 구현해도, 비교하는데 있어서 의도한 결과를 얻어낼 수 있다.



### HashCode와 Equals

`eqauls()`만으로도 비교가 가능하면 `HashCode()` 는 왜 동반되어야 하는지 궁굼해 졌다.

결론은 동일한 객체라면, 동일한 해시코드를 가져야하기 때문에 필요하다. HashCode를 통해 `HashMap`, `HashSet`, `HashTable` 을 사용하는 경우 불이익을 받을 수 있기 때문이다. 

```java
public void play() {
  Person Royce1 = new Person("Royce", 23);
  Person Royce2 = new Person("Royce", 23);
  System.out.println(Royce1.equals(Royce2)); // true
  
  Set<Person> personSet = new HashSet<>();
  personSet.add(Royce1);
  personSet.add(Royce2);
  
  System.out.println(personSet.size()); // 2
}
```

`Royce1` 과 `Royce2` 의 `equals()` 는 `true`이다. 하지만, 중복을 허용하지 않는 HashSet에 추가하였더니 2개 모두 정상적으로 추가되었다. 

이런 경우에서 예상치 못한 오류들이 발생할 수 있기 때문에 HashCode와 Equals의 관계를 통해, "동일한 객체라면 동일한 HashCode를 가져야 한다"를 충족해야 한다.

- Override `HashCode()` 

```java
@Override
public int hashCode() {
  return Objects.hash(name, age);
}
```

이제 위 조건을 만족하였기 때문에 객체를 동일하게 만들기 위해 필요한 작업을 모두 마쳤다.

- 종합 코드

```java
class Person {
	String name;
	int age;

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public boolean equals(Object obj) {
		if (!(obj instanceof Person)) {
			return false;
		}
		Person person = (Person)obj;

		return this.age == person.age
			&& this.name.equals(person.name);
	}

	@Override
	public int hashCode() {
		return Objects.hash(name, age);
	}

}

public void play() {
  Person Royce1 = new Person("Royce", 23);
  Person Royce2 = new Person("Royce", 23);
  System.out.println(Royce1.equals(Royce2)); // true
  
  Set<Person> personSet = new HashSet<>();
  personSet.add(Royce1);
  personSet.add(Royce2);
  
  System.out.println(Royce1.hashCode()); // -1841162118
  System.out.println(Royce2.hashCode()); // -1841162118
  System.out.println(personSet.size()); // 1

  System.out.println(System.identityHashCode(Royce1)); //197964393
  System.out.println(System.identityHashCode(Royce2)); //1620890840
}

```

`HashCode()` 를 오버라이딩하였기에 `HashSet()` 에도 중복이 제거되어 저장되었다. hashCode를 출력해보아도 동일하다는 것을 알 수 있다.

추가로, `System.identityHashCode()` 를 통해 오버라이딩 작업 이전의 HashCode를 확인 할 수 있다.
# ORM (Object Relational Mapping)

### 객체지향과 관계형 데이터 베이스

현대의 개발언어, Java, Swfit, Python, C++, PHP 등 다양한 언어들은 객체라는 패러다임을 가지고 등장하였다. 그리고 이 언어를 활용하여 관계형 데이터베이스에 접근하여(by SQL) 데이터를 저장하고 다룬다.

객체지향언어는 유지보수에 유리하고 생산성이 향상된다는 장점, 인간 개발자에게 자연스로운 모델링 덕분에 자주 사용되며
관계형 데이터베이스는 분류, 정렬, 속도에 유리하고 무결성을 보장해주기에 데이터베이스에 적합하게 사용되고 있다.

상태와 행동(메서드)을 가진 객체라는 독립된 단위가 상호작용하는 프로그래밍을 지향하는 언어,
그리고 키와 값들의 관계를 테이블화 시킨 관계형 데이터 베이스의 직접적인 불일치는 발생할 수 밖에 없다.

간단하게, 객체지향의 상속을 살펴보자. A가 B를 상속한 경우, 관계형 데이터베이스는 상속관계를 알아 듣지 못한다. 그래서 Tabel A와 Table B를 각각 생성하고, Key를 통해 관계를 생성한다. 이 자체만으로도 불일치가 느껴지는 상황인데, 여기서 A를 상속한 또다른 객체 C가 등장했다고 하자. SQL로 조작할 수 있는 데이터베이스는 전혀 객체와 비슷하게 동작하지 못한다.

#### 이러한 패러다임의 불일치를 해결하기 위해 등장한 기술이 **ORM**이다



## ORM이란

객체-관계 매핑

객체 지향 프로그래밍의 Java와 RDBMS인 sql에서는 모델간의 불일치가 발생 할 수 밖에 없다. 이러한 차이를 자동으로 매핑(연결)해주는 것을 ORM이라고 한다.

객체 모델과 관계형 모델 간의 불일치를 해결해준다는 것은 객체지향 프로그래밍에 더 집중할 수 있게 도와준다.

개발자는 객체-관계라는 개념을 하나로 생각하거나 고민할 필요가 없어진다. 객체는 객체대로, 관계형은 관계형대로 설계를 한 뒤 ORM을 통해 매핑하면 되기 때문이다.

자바 진영에서 ORM을 수행하는 기술 중 하나가 JPA이다.



### 영속성 (Persistence)

> 프로그램이 실행 된 뒤 데이터를 생성하게 되면 메모리에 존재하게 되는데, 이 데이터는 프로그램을 종료하면 모두 없어진다. 이러한 데이터들이 데이터베이스에 담기면 프로그램의 여부와 관계없이 존재할 수 있고, 이러한 특성을 영속성이라고 한다

### Persistence Layer

> 프로그램 구조에서 데이터들에게 영속성을 부여(데이터를 저장)해주는 계층을 말한다. ex) JDBC, JPA, MyBatis

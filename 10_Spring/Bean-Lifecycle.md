# Bean Lifecycle

`Spring Container`는 `Bean`객체들을 관리한다. 여기서 관리란 `Bean`객체의 생성과 소멸, 의존성 주입등을 담당하는데, 이를 **생명주기(Lifecycle)**를 관리한다고 한다,



### Bean Lifecycle

`Spring Container`가 `Bean`의 생명주기를 관리하는 흐름에 대해 간단히 살펴보면,

프로그램 실행이 `Spring Container`실행되고, 각각의 요청에 맞는 Bean의 인스턴스들이 생성된다. 그 후 DI(Dependency Injection)이 이뤄진다. 그 후, `Bean`은 `Spring Container`가 닫힐 때 소멸된다.


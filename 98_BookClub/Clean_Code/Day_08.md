# 2022.02.02

## 📗 오류 처리

### 읽은 페이지 : (p.130 ~ p.142)

>### 😃 책에서 기억하고 싶은 내용 

- 하지만, 오류 처리 코드로 인해 프로그램 논리를 이해하기 어려워진다면 깨끗한 코드라 부르기 어렵다. (p.130)
- 먼저 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하는 방법을 권장한다. 그러면 자연스럽게 try블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션 본질을 유지하기 쉬워진다. (p.133)
- 호출자를 고려해 예외 클래스를 정의하라 (p.135)

<br>

<br>

>### ☺️ 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적기

- 오류 처리 보단, 예외를 던지자. 우아한 테크코스 프리과정을 진행하면서 예외를 처리하는 로직을 어디서 구현 할 지 고민해왔다. 예외처리에 대한 이해가 얕아서 인것 같았다. 예외는 코드의 일부분이고 하나의 역할을 수행한다. 

<br>

<br>

>### 🔎 궁금한 내용이 있거나, 잘 이해되지 않는 내용 

- 

<br>

<br>

> ### ✍️  소감 3줄 요약 

- 호출자를 고려해 예외 클래스를 정의하라.
- 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하자.
- 예외 처리는 하나의 역할이고 코드이다.
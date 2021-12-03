# assertTimeout

```java
import org.junit.jupiter.api.Assertions;

static void assertTimeout(Duration timeout, Executable executable, String message?)
// ?는 옵셔널
```

- timeout : 주어진 시간
- executable : 실행할 assert문

> `assertTimeout()`은 주어진 시간안에 `executable`이 작동하는지 테스트 하는 것.

```java
// 성공 예시
@Test
public void Test_1(){
	assertTimeout(ofMinutes(2), () -> {}}); 
}
//PASS

// 실패 예시 
@Test
public void Test_2(){
  assertTimeout(ofMillis(10), () -> {
    Thread.sleep(100);
  });
}
//FAIL
```



`assertTimout()`의 작동을 보면

`executable`이 실행되기 전에 시간측정을 시작하고, 

`executable`이 종료된 시점에 시간을 측정하여

`timeout`와 비교하여 테스트 결과를 반환다.

```java
@Test
public void Test_2(){
  assertTimeout(ofMillis(1000), () -> {
    Thread.sleep(10000);
  });
}
//FAIL
```

이러한 작동과정 때문에, 위 코드의 테스트 결과를 알기 위해선 1000ms가 아닌 10000ms를 기다려야 한다.



이러한 불편함을 보완해주는 또 다른 assert메서드가 존재한다.

#### assertTimeoutPreemptively

```java
import org.junit.jupiter.api.Assertions;

static void assertTimeoutPreemptively(Duration timeout, Executable executable, String message?)
//?는 옵셔널  
```

`assertTimeoutPreemptively`는 `asssertTimeout`과 기능은 동일하지만, 다른 과정을 거치는 덕분에 아래 코드의 테스트 결과를 100ms만에 알 수 있다.

```java
@Test
public void Test_2(){
  assertTimeoutPreemptively(ofMillis(100), () -> {
    Thread.sleep(10000);
  });
}
//FAIL
```


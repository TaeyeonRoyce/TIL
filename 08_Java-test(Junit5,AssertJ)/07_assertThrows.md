# assertThrows

```java
static <T extends Throwable> T assertThrows(Class<T> expectedType, Executable executable, String message)
```

- expectType: 예측할 ExceptionType

- executable: 실행할 assert문들

```java
@Test
public void Test_1() {
  assertThrows(ArithmeticException.class, () -> calculator.divide(1, 0));
}
//PASS
```


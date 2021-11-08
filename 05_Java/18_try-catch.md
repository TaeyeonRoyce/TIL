# 예외의 처리

예외는 프로그래머가 코드를 통해 수정이 가능한 오류를 말한다.

어떻게 수정을하고 처리하는지 알아보자.



### 예외의 생성

```java
int a = 10 / 0; //--1
/*
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at playground.playGround.play(playGround.java:6)
	at playground.main.main(main.java:6)
*/
```

런타임 에러가 발생하면, 어떤 에러이고 어디서 발생하는지 알려준다.

에러가 발생하면, 그에 해당하는 에러(예외)클래스 타입의 인스턴스가 생성된다. 그 인스턴스에는 에러에 대한 정보와 메서드를 가지고 있기 때문에 예외가 발생한다고 해서 무조건 종료되는 것은 아니다.



### try-catch

try-catch를 통해 에러를 처리할 수 있다.

```java
public class playGround {
    public static void play() {
        try{
            System.out.println(1+1);
            System.out.println(1/0);  //--1
          	System.out.println(1+2);  //--3
        }catch (ArithmeticException e){ //--2
            System.out.println("ArithmeticException");
        }
    }
}
// result
// 2
// ArithmeticException
```

`--1`을 보면, `ArithmeticException` 타입의 인스턴스가 생성된다. 이때 인스턴스가 생성된 문장이 try블럭안에 있다면, 이를 처리할 수 있는 catch블럭을 찾아간다.

`--2`에서 `ArithmeticException`를 처리하고 있기 때문에 해당 구현부를 수행하고, try-catch문을 빠져나온다. 그래서 `--3`의 문장은 실행되지 않는다.

```java
public class playGround {
    public static void play() {
        try{
            System.out.println(1+1);
            System.out.println(1/0);
            System.out.println(1+2);
        }
        catch (Exception e){ //--1
            System.out.println("Exception");
        }catch (ArithmeticException e){ //--2
            System.out.println("ArithmeticException");
        }
    }
}
```

catch문도 주의해야할 점이 있다. `--1`의 `Exception`타입은 `--2`의 `ArithmeticException`의 부모이다. 그래서, try문에서 발생한 에러는 `--1`에서도 잡아낼 수 있다.(다형성)

try-catch문의 흐름을 이해했다면, 더 넓은 범위의 예외타입이 아래쪽에 위치해야 좀 더 세부적인 예외처리가 가능하다는 것을 쉽게 알 수 있다.

### finally

```java
public class playGround {
    public static void play() {
        try{
            System.out.println(1/0);
        }catch (Exception e){ //--1
            System.out.println("Exception");
        }finally{
            System.out.println("finished");
        }
    }
}
```

예외의 발생여부와 상관없이 수행하고자 하는 문장은 `finally`블럭을 통해 실행 할 수 있다.



### 예외 클래스의 메서드

```java
public class playGround {
    public static void play() {
        try{
            System.out.println(1+1);
            System.out.println(1/0);  
            System.out.println(1+2);
        }
        catch (ArithmeticException e){ 
            e.getMessage();
            e.printStackTrace();
        }
    }
}
```

예외클래스들은 여러 메서드를 가지고 있기 때문에, 이를 활용해서 생성된 예외 인스턴스를 다양하게 다룰 수 있다.



### 예외 발생시키기

```java
public class playGround {
    public static void play() {
        try{
            throw new ArithmeticException("ArithmeticException!!");
        } catch (ArithmeticException e){
            System.out.println(e.getMessage());  //ArithmeticException!!
        }
    }
}
```

throw를 통해 예외를 고의적으로 발생 시킬 수 있다.

`throw new Exception()`에서 매개변수는 `getMessage()`메소드와 연관있다는 것을 알 수 있다.



### 멀티 catch블럭

```java
try{
   System.out.println(1/0);
 }
catch (ArithmeticException e){ 
  System.out.println("Error");
}
catch (NullPointerException e){ 
  System.out.println("Error");
}
```

catch문의 중복을 제거할 수 있다.

```java
try{
   System.out.println(1/0);
 }
catch (ArithmeticException | NullPointerException e){ 
  System.out.println("Error");
  //e.methodOnlyFromNullPointerEx(); ERROR  --1
}
```

두 코드는 동일한 코드이다. 이때 주의해야 할 점은, 

- 다른 타입의 참조변수임을 고려하여 구현해야 한다는 점, `--1`
- 부모 자식간의 멀티 블럭은 허용되지 않는다는 점이다.






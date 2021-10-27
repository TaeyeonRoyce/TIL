# Java 프로그램

- 자바의 모든 코드는 반드시 **클래스** 안에 존재하여야 하며,
- 관련된 코드들이 별도의 클래스들을 이루고, 이 클래스들이 모여 Java 어플리케이션을 이룬다.

- Java 어플리케이션은 main 메서드를 포함한 클래스가 반드시 하나 있어야 한다.
-  `java.exe`가 `main`메서드를 호출하여 Java 어플리케이션을 실행한다.

```java
class Hello{
		public static void main(string[] args){
      System.out.println("Hello, world.");
    }
}
```



Java 어플리케이션은 여러 소스파일들이 구성한다.ㄴ

하나의 소스파일내에 하나의 클래스를 정의하는 것을 권장한다.\

또, 소스파일내에 `public class`가 존재하는 경우, 소스파일의 이름은 반드시 `public class`의 이름과 동일해야 하며, `public class`는 반드시 하나만 존재하여야 한다

```java
//ex)
//Hello.java
public class Hello{
  // ...
} 

//hello.java
public class Hello{
  // ...
} //Wrong
```

 


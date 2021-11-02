# Import

import를 통해 현재 소스파일 이외의 클래스나 패키지에 접근이 가능하다.

import를 하지 않고도 직접 명명하며 사용할 수 있지만, 매번 그렇게 사용한다면 불편할 것이다.

```java
// main.java

import pkg1.importExClass;

public class main {

    public static void main(String[] args){
        importExClass ex = new importExClass();
        pkg1.importExClass ex2 = new pkg1.importExClass(); //import가 없다면,
        System.out.println(ex.x);  //10
        System.out.println(ex2.x); //10
    }
}
```

```java
// importExClass.java
package pkg1;
public class importExClass {
    public int x = 10;
    public class importExClass2 {
        public void printMessage(){
            System.out.println("Here is importExClass");
        }
    }
}
```

import를 통해 다른 패키지에 존재하는 클래스를 보다 간결하고 명확하게 접근할 수 있게 되었다.

import를 선언하는 방법에 따라 그 표현을 더욱 명확하게 할 수 있다.

- `import package.class` : `package`라는 패키지에 `class`라는 클래스를 사용

- `import package.*`: `package`라는 패키지에 모든 클래스를 사용

- `static import` : static멤버를 호출할 때 클래스 이름을 생략 할 수 있다.

  ```java
  import static java.lang.System.out;
  //System.out.println("Hello world");
  out.println("Hello world");
  ```

소스파일을 만들때, 선언한적 없은 클래스(`String()`, `Systme.out.println()`) 를 사용할 수 있는 이유는, 기본적으로 `import java.lang.*`이 되기 때문

> 패키지는 클래스들의 묶음이다.
>
> 프로젝트내의 디텍토리 같은 역할을 한다.
>
> ' . '을 통해 계층구조를 나타낼 수 있고, 하나의 소스파일엔 한 번만 패키지 선언이 가능하다


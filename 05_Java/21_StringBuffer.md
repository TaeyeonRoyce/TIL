# StringBuffer

`String`클래스와 유사하게 문자열을 저장할 수 있지만, 변경에 용이한 클래스이다.

`char[]`를 좀더 유연하게 사용하여 문자 편집에 용이하다. 기본적인 크기(16)가 존재하고, 필요에 맞게 조절할 수 있다.

### 생성자

```java
public class playGround {
    public static void play(){
        StringBuffer sb = new StringBuffer("abc"); //--1
        System.out.println(sb.capacity()); //19
        System.out.println(sb.length());   //3
      	StringBuffer sb2 = new StringBuffer(10);   //--2
      	System.out.println(sb2.capacity()); //10
    }
}
```

`StringBuffer` 인스턴스를 만들어야 한다.

이때, 문자열을 주면`(--1)` 예상대로 동작한다. 문자열의 길이에 추가적으로 기본크기인 16이 추가되어 문자열을 담은 `char[]`의 길이가 19(16 + 3)이 된다.

`(--2)`처럼 정수값을 주면, 기본값이 아닌 원하는 길이의  `char[]`이 생성된다.

`.capacity()`와 `.length()`의 차이도 코드를 통해 확인할 수 있다.



### StringBuffer 메서드

- `StringBuffer append()` : 매개변수로 입력된 값을 문자열로 변환하여 맨 뒤에 추가한다

  ```java
  public class playGround {
    public static void play(){
      StringBuffer sb = new StringBuffer("abc");
      sb.append(123);
      sb.append(true);
      System.out.println(sb);  //abc123true
    }
  }
  ```

- `StringBuffer delete(int start, int end)` : `start ..< end`까지의 문자를 제거한다

  ```java
  sb.delete(3, 6);
  System.out.println(sb); //abctrue
  sb.deleteCharAt(3); // 위치가 3인 문자 제거
  //abcrue
  ```

- `StringBuffer insert(int pos, any)`: `pos`위치에 `any`를 삽입한다

  ```java
  sb.insert(3,"t");
  //abctrue
  ```

- `StringBuffer reverse()`: 배열을 뒤집는다

  ```java
  sb.reverse();
  //eurtcba
  ```

- `StringBuffer setLength` : 배열의 길이를 변경한다

  ```java
  public class playGround {
    public static void play(){
      StringBuffer sb = new StringBuffer("abcdefg");
      sb.setLength(5);
      System.out.println(sb); //abcde (fg는 짤림)
      sb.setLength(12);
      System.out.println(sb); //abcde      (남은 공간은 공백으로 채워짐)
      System.out.println(sb.charAt(8)); // 공백
    }
  }
  ```



### StringBuilder

`StringBuffer`와 동일한 기능을 가진 클래스이다.

쓰레드의 동기화 유무이다. 멀티쓰레드로 작동하는 경우, 데이터들간의 동기화가 필요한데 `StringBuffer`는 동기화가 자동으로 된다. 그래서 싱글쓰레드로 작동하는 프로그램의 경우에는 불필요한 성능이 될 수 있다. 큰 차이는 없을 수 있지만, 프로그램에 따라 성능향상을 고려해본다면 `StringBuilder`와 `StringBuffer`의 차이를 알아두는 것이 좋을 것 같다.
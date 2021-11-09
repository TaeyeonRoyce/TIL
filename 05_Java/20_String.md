# String

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
//  ...
}
```

`String`은 문자형 배열을 저장하는 클래스이며, 이를 다룰수 있는 여러 메서드들을 제공한다.

위 코드처럼 `String`은 변경 불가능한 클래스이다.

```java
String a = "A";
String b = "B";
a = a + b; //--1
```

이렇게 하면 `A` -> `AB`로 변하는 것 같지만, 실제 동작은 변경하지 않는다. `--1`의 수행 과정에서, 새로운 객체 `AB`라는 새로운 인스턴스가 생성되고, `a`가 더이상`A`가 아닌 `AB`를 가리키게 되는 것이다.

`String`클래스의 이러한 특성 때문에 기본형(int, double, char, ...)와 동일한 방법으로 작동하지 않는다. 보이기엔 비슷하게 작동하지만, 효율성을 높히고자 한다면 동작원리에 대해 알 필요가 있을 것이다.

```java
String a1 = "A";  //--1
String a2 = "A";
String a3 = new String("A"); //--2
String a4 = new String("A");
```

`--1`과 `--2`의 결과는 같겠지만, `--1`의 방식이 권장된다. 프로그램이 동작하면, `--1`에 존재하는 `A`는 `constant pool`이라는 곳에 저장되고, 이를 재사용한다. 그래서 `a1`과 `a2`는 같은 인스턴스(?)를 가리키지만, 생성자`(--2)`를 통해 생성하면, `a3`와 `a4`가 각각 새로운 인스턴스를 생성하기 때문에 성능면에서 조금 떨어질 수 있다.

## String 생성자

- ### 생성자

  `String(String s)` 

  `String(char[] value)` : char의 배열을 String으로 만듦

  ```java
  char[] ab = {'A','B'};
  String s = new String(ab); //AB
  ```

  `String(StringBuffer value)` : StringBuffer를 String으로 만듦

  

## String 메서드

- ### 비교

  `int compareTo(String str)` : `str`과 사전순서로 비교하여 결과 반환, 정렬때 유용할 듯

  ```java
  int i = "a".compareTo("b"); // -1  a < b 
  int i = "b".compareTo("a"); // 1   b > a
  int i = "aa".compareTo("aa"); // 0  aa == aa
  ```

  

  `boolean equals(Object obj)` : 일치하는지 비교

  ```java
  String a = "A";
  boolean same = a.equals("A"); //true
  boolean diff = a.equals("a"); //false
  
  //대소문자 구분 없이 비교할 경우
  boolean ignoreCase a.equalsIgnoreCase("a"); //true
  ```

  

- ### 검사

  `boolean contains(CharSequence s)` : `s`를 포함하는지 검사

  ```java
  String a = "ABC";
  boolean b = a.contains("AB"); //true
  ```

  

  `int indexOf(int ch)` : `ch`의 위치를 반환

  ```java
  String a = "HelloolleH";
  int h1 = a.indexOf("H");    // 0,    맨 앞에서부터 찾음
  int h2 = a.indexOf("H", 3); // 9,    3의 위치부터 찾음
  int none = a.indexOf("K");  // -1,   없으면 -1
  int s1 = a.indexOf("ll");   // 3    
   
  int h3 = a.lastIndexOf("H");// 9,    뒤에서부터 찾아서 위치 반환
  
  char loc = a.charAt(9);     //H,     9에 있는 문자를 반환
  ```

  

  `boolean startsWith(String str)` : `str`로 시작하는지 확인

  ```java
  String a = "HelloWorld";
  boolean starts = a.startWith("Hel"); // true
   
  boolean ends = a.endsWith("ld"); //true 끝나는 문자 확인
  
  ```

  `int length()` : 길이 반환

  

- ### 슬라이싱

  `String substring(int begin)` : `begin`부터 나눈 문자열 반환

  ```java
  String a = "HelloWorld !!!";
  String b = a.substring(5);   //World !!!
  String c = a.substring(0,5); //Hello  0~4까지
  ```

  `String trim()`: 양옆 공백 제거

  ```java
  String a = "  Hello ab  ";
  String b = a.trim();		//Hello ab
  ```

  `String[] split(String regex)` : `regex`로 나누어 문자열 배열로 담기

  ```java
  String a = "A,B,C";
  String[] aArr = a.split(",");
  //aArr[0] = A
  //aArr[1] = B
  //aArr[2] = C
  
  //join을 통해 결합하여 문자열로 만들 수 있다
  String aAgain1 = String.join(",", aArr);
  // A,B,C
  
  String aAgain2 = String.join("-", aArr);
  // A-B-C
  ```

  `String concat(String str)` : `str`을 붙히기

  ```java
  String a = "A";
  String b = "B";
  String ab = a.concat(b); //AB
  ```



- ### 다루기

  `String toLowerCase()` : 소문자로,

  `String toUpperCase()` : 대문자로

  

  

## String과 형변환

### 기본형을 문자열로 변환 하는 경우 :`String.valueOf()`를 통해 변환 가능

```java
public class playGround {
    public static void play(){
        int integer = 10;
        char character = 'A';
        boolean isTrue = true;
        double doubleNumber = 10.12;
        System.out.println(String.valueOf(integer));      //10
        System.out.println(String.valueOf(character));    //A
        System.out.println(String.valueOf(isTrue).toUpperCase());       //TRUE
        System.out.println(String.valueOf(doubleNumber)); // 10.12
      
      
      	String a = number + "";
      	String b = String.valueof("10");
      //동일한 결과지만, valueOf의 성능이 더 우세함
    }
}
```



반대의 경우도 `valueOf()`를 사용하지만, 변환하려는 타입에 맞는 래퍼클래스를 통해 사용해야한다.

```java
public class playGround {
    public static void play(){
        String number = "10";
        String doubleNumber = "10.12";
        System.out.println(Integer.valueOf(number) + Double.valueOf(doubleNumber)); //20.119999999999997
      //int와 double의 연산
     		int a = number + "";
      	int b = Integer.valueof("10");
    }
}
```


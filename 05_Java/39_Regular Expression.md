# Regular Expression

Java에서는 `java.util.regex` API의 `Pattern`클래스와 `Matcher`클래스를 활용하여 정규표현식을 작성할 수 있다.

### Pattern Class

- `complie()`메서드를 통해 주어진 `regex`(String)을 정규 표현식 패턴으로 만든다.

- `matches()`메서드를 통해 검증한다.

```java
public final class Pattern implements java.io.Serializable {
  public static boolean matches(String regex, CharSequence input) {
          Pattern p = Pattern.compile(regex); 
          Matcher m = p.matcher(input);
          return m.matches();
      }
}
```

-> Matcher Class의 `matches()`를 반환한다.

- 예시 코드

```java
import java.util.regex.Pattern;

public class playGround {

    public void play() {
        String regex = "^[0-9]*$"; //숫자만
        String target = "123456789"; //대상 문자열

        System.out.println(Pattern.matches(regex, target));
    }
}
//true
```



### Matcher Class

`Pattern` 클래스로 부터 생성된 정규 표현식 패턴을 해석하고 검증하는 작업을 수행함

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class playGround {

    public void play() {
        Pattern pattern = Pattern.compile("^[a-zA-Z]*$"); //영문자만
        String val = "abcdef"; //대상 문자열

        Matcher matcher = pattern.matcher(val);
        System.out.println(matcher.matches());
    }
}
```



### 정규 표현식 사용 이유

Java에서 사용하는 가장 큰 목적은 아마 사용자의 입력에 대한 검증일 것이다.

만약 지정된 형식으로 입력을 받고자 한다면, 무수히 많은 경우의 입력에 대비하여 지정된 형식이 맞는지 확인하기 위해 많은 조작과정이 필요할 것이다. 하지만, 정규 표현식을 통해 지정된 형식을 저장하고, 입력값이 이 형식과 맞는지 아닌지만 판단한다면, 손쉽게 처리할 수 있을 것이다.

```java
package playground;


import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class playGround {

    public void play() {
        String userName = "홍길동";
        String phoneNum = "010-1111-1111";
        String emailAddress = "regex@gmail.com";

        String koreanRegex = "^[가-힣]*$";
        String phoneNumberRegex = "^01(?:0|1|[6-9])-(?:\\d{3}|\\d{4})-\\d{4}$";
        String emailRegex = "\\w+@\\w+\\.\\w+(\\.\\w+)?";

        System.out.println(Pattern.matches(koreanRegex, userName)); //true
        System.out.println(Pattern.matches(phoneNumberRegex, phoneNum)); //true
        System.out.println(Pattern.matches(emailRegex, emailAddress)); //true

        String unMatchedEmailAddress = "re@gex@gmail.com";

        System.out.println(Pattern.matches(unMatchedEmailAddress, emailAddress)); //false
    }
}

```

대표적으로 많이 사용되는 전화번호와 이메일에 대한 유효성 검증을 해보았다. 사용자 입력에 대한 검증이 훨씬 간단해졌을 뿐만아니라 지정된 형식이 변경되어도 이를 검증할 로직의 수정이 아닌, 형식에 대한 정규 표현식만 변경하면 되므로 수정에 있어서도 유연하게 대처할 수 있게 되었다.

정규 표현식을 통해 얻는 이점이 있지만, 이를 이해하고 사용하려면 숙련된 기술이 요구된다. 정규 표현식에 대한 공부를 병행하며 사용자 입력에 대한 검증을 수행하도록 하자.
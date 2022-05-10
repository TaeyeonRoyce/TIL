# JWT

### Json Web Token

> Json 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token

Connectionless, Stateless 특징을 가지는 HTTP를 활용하 통신을 하는 경우, 상태를 유지해야 할 경우가 존재한다.
이를 위한 방안으로 쿠키나 세션을 활용하여 로그인과 같은 상태를 유지하여 통신하기도 하고,
또 다른 방법으로 해당 유저(상태)에 대한 토큰을 발급하여 토큰에 담겨있는 정보를 통해 DB를 조회하지 않고 어떤 요청의 상태를 알아 낼 수 있다.

토큰은 어떤 값이나 ID를 통해 해당 리소스를 조회하는 것이 아닌 토큰 자체에 상태(정보)가 포함되어 전달된다는 특징이 있다. (Self-Contained 방식)

## JWT 구성요소

![img](https://github.com/KoEonYack/Tistory-Coveant/blob/master/Article/WebTech/jwt%EB%9E%80/img/c_2.PNG?raw=true)

JWT는 Header, Payload, Signature의 3부분으로 이루어지며, Json포맷인 각 부분은 Base64Url로 인코딩 되어 표현된다. 각 부분은 `.` 으로 구분되어있다.

```
구조: [Base64(HEADER)].[Base64(PAYLOAD)].[Base64(SIGNATURE)]
```

> Base64 : 이진 데이터를 ASCII영역의 문자로 바꾸는 인코딩 방식

- ### **Header**

  JWT 토큰의 해석 방식이 저장되어 있음

  ```json
  { 
          "alg": "HS256",
          "typ": JWT
  }
  ```

  → `"alg"` : Signature를 해싱하는 알고리즘

  → `"typ"` : 토큰의 타입

- ### **PayLoad**

  토큰에 대한 정보가 저장되어 있음

  Key - Value 쌍으로 이루어져 있고, 이 정보 단위를 **클레임**이라고 한다.

  클레임은 크게 `Registered Claims`, `Public Claims`, `Private Claims` 로 구분된다.

  - Registered Claims (등록된 클레임) : 토큰 정보를 표현하기 위해 이미 정해진 종류의 데이터

    JWT를 간결하게 하기 위해 key는 모두 길이 3의 String이다. 여기서 subject로는 unique한 값을 사용하는데, 사용자 이메일을 주로 사용한다고 함.

    - iss: 토큰 발급자(issuer)
    - sub: 토큰 제목(subject)
    - aud: 토큰 대상자(audience)
    - exp: 토큰 만료 시간(expiration), NumericDate 형식으로 되어 있어야 함 ex) 1480849147370
    - nbf: 토큰 활성 날짜(not before), 이 날이 지나기 전의 토큰은 활성화되지 않음
    - iat: 토큰 발급 시간(issued at), 토큰 발급 이후의 경과 시간을 알 수 있음
    - jti: JWT 토큰 식별자(JWT ID), 중복 방지를 위해 사용하며, 일회용 토큰(Access Token) 등에 사용

  - Public Claims (공개 클레임)

    사용자 정의 클레임으로, 공개용 정보를 위해 사용. 충돌 방지를 위해 URI 포맷을 이용한다고 한다.

  - Private Claims (비공개 클레임)

    서버와 클라이언트 사이에 임의로 지정한 정보를 저장하기 위해 만들어진 사용자 지정 클레임

- ### **Signature**

  토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드

  위에서 만든 Header와 Payload의 값을 각각 BASE64로 인코딩 한 뒤, 비밀 키를 이용해 Header에서 정의한 알고리즘으로 해싱을 하고, 이 값을 다시 BASE64Url로 인코딩하여 생성 ⇒ Signature

  Signature에는 Header와 Payload의 인코딩 한 값에 추가로 서버에서만 알고있는 값(키)를 추가하여 Header에 명시된 알고리즘을 통해 해싱한 뒤 인코딩한 값이 저장된다.

  해싱 알고리즘은 단방향으로 이루어지기 때문에 Signature의 정보를 디코딩해도, 서버에서만 알고있는 값(키)를 알아낼 수 없다.

서버는 사용자 요청에 JWT가 존재하면, Header와 Payload의 값 + 서버의 비밀 키와 함께 알고리즘을 돌려 계산된 해싱 값을 Signature와 비교(+유효기간 확인)하여 인가한다.



## JWT의 장점과 단점

### 장점

- JWT의 주요 이점은 사용자 인증에 대한 정보가 토큰 자체에 담겨 있다 보니 별도의 인증 저장소가 필요하지 않다.
- 쿠키를 전달하지 않아도 되므로 쿠키를 사용함으로써 발생하는 취약점이 없다.
- URL 파라미터와 헤더로 사용
- 트래픽 대한 부담이 낮음

### 단점

- JWT의 토큰 길이는 제한이 없어, 정보가 많아질수록 네트워크 부하가 이루어짐
- Self-Contained라 별도의 인증 저장소는 필요하지 않지만, 이 자체에 모든 정보가 담겨 있다는 점은 보안 부분에서의 부담을 줄 수 있음
- Payload는 누구나 디코딩 할 수 있기에, 중요한 정보는 다른 암호화를 하거나 넣지 못함
- Token의 제어가 불가능함. 발급한 토큰을 임의로 삭제할 수 없음





이미지 출처 : https://covenant.tistory.com/201
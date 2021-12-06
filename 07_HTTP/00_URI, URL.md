# URI (Uniform Resource identifier)

### Uniform : 리소스를 식별하는 통일된 방식

### Resource : 자원, URI로 식별할 수 있는 모든 것

### Identifier : 다른 항목과 구분하는데 필요한 정보

> URI는 결국 식별가능한 자원을 구분가능하게 지칭되어있는 것

URI에는

- 리소스의 위치를 지정하는 URL `(Locator)`
- 리소스에 이름을 부여하는 URN `(Name)`

이 있다.

이름만으로 무수히 많은 리소스들을 식별하는 것은 쉽지않기에, 대부분 URL을 사용한다.



### URL의 문법

> scheme://[userinfo@]host\[:port]\[/path]\[?query][#fragment]

>https://www.google.com:443/search?q=hello&hl=ko

- scheme

  주로 프로토콜 사용 (https프로토콜은 포트번호는 생략 가능)

- host

  주로 도메인 명이 들어감

- /path

  호스트에 존재하는 리소스의 경로

- query

  key,value 형태의 웹서버에 제공하는 파라미터

  
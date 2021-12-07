# HTTP Header

### 표현

HTTP 표준이 RFC2616 -> RFC 7230이 되면서, 기존에 사용하던 엔티티라는 용어 대 표현(Representation)이라는 용어를 사용한다.

메세지 본문에 담긴 데이터가 HTML, JSON등 추상적인 리소스에 대하여 여러 형태로 표현될 수 있기 때문에 표현이라는 용어가 사용됬다고 한다.



### 표현 헤더

- Content-Type : 표현 데이터의 형식

  >표현 데이터에 대한 내용의 형식을 설명함
  >
  >- **application/json**
  >- image/png
  >- text/html; charset-utf-8

  

- Content-Encoding : 표현 데이터의 압축 방식

  >표현 데이터의 압축정보
  >
  >데이터를 읽는 쪽에서 이 헤더의 정보로 압축을 해제한다
  >
  >- deflate
  >- gzip
  >- identity

  

- Content-Language : 표현 데이터의 자연 언어

  > 표현 데이터의 자연 언어에 대한 정보
  >
  > - ko
  > - en

  

- Content-Length : 표현 데이터의 길이

  > 바이트 단위의 길이 정보



### 협상 헤더 (Negotiation)

클라이언트가 요청시 표현에 대한 선호

Quality Values(q)에 대하여 0~1사이의 값을 사용하여 선호를 표현. default = 1, 큰 값이 우선순위를 가짐

- Accept : 클라이언트가 선호하는 미디어 타입 전달

  >Quality Values(q), 구체적인 것이 우선
  >
  >Accept: text/*, text/plain, text/plain;format=flowed, \*/\*
  >
  >1. text/plain;format=flowed
  >2. text/plain
  >3. text/*
  >4. \*/*

  

- Accept-Charset : 클라이언트가 선호하는 문자 인코딩

- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩

- Accept-Language : 클라이언트가 선호하는 자연 언어

  > Accept-Language : ko-KR, ko;q=0.9,en-US;q=0.8
  >
  > 1. ko-KR
  > 2. ko
  > 3. en-US



### Host **(필수)**

요청한 호스트 정보(도메인) 



### Location

페이지 리다이렉션

웹 브라우저는 300~ 의 HTTP상태코드를 가진 응답결과에 대해 Location 헤더를 통해 리다이렉팅함



### Allow

허용 가능한 HTTP 메서드



### Referer (Referrer X)

현재 요청된 페이지의 이전 웹 페이지 주소를 나타냄

Referer를 사용해서 유입 경로를 분석 할 수 있다.



### User-Agent

클라이언트의 어플리케이션 정보를 나타냄 (ex.웹 브라우저)

브라우저별 오류 분석 가능



### Server

origin서버의 정보를 나타냄



### Date

메세지가 발생한 날짜와 시간


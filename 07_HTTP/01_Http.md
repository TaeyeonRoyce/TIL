# HTTP

HyperText Transfer Protocol

### 기반 프로토콜

TCP : **HTTP/1.1**, HTTP/2

UDP : HTTP/3



### 특징

- ##### 클라이언트 서버 구조

  > 클라이언트의 Request와 서버의 Response 구조
  >
  > 요청을 보내고, 응답을 대기한다.
  >
  > 클라이언트와 서버의 분리로 각각의 발전에 영향을 많이 주었다고함



- ##### 무상태 프로토콜

  >Stateless (무상태)
  >
  >어느 응답 서버도 대응이 가능하다.
  >
  >스케일 아웃- 수평확장에 유리

- ##### 비연결성

  >불필요한 경우도 연결을 끊어 자원사용을 줄일 수 있다.
  >
  >매번 연결이 필요하다는 단점이 있다.(3hand shake 시간 소모)

  

- ##### HTTP 메세지

  > 요청, 응답 메세지 존재
  >
  > - 메세지 구조
  >
  >   HTTP-message = 
  >
  >   ​	start-line
  >
  >   ​	*( header-field CRLF ) *(http header)*
  >
  >   ​	CRLF *(공백)*
  >
  >   ​	[ message-body ]			

- ##### 단순, 확장 가능

  >HTTP와 HTTP메세지도 매우 단순하다.
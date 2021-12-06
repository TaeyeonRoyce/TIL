# HTTP 메세지



### 메세지 구조 

### HTTP-message = 

- ##### start-line

- ##### *( header-field CRLF ) *(http header)*

- ##### CRLF *(공백)*

- ##### [ message-body ]	



### start-line

- ##### 요청 메세지

  >**GET /search?q=hello&hl=ko HTTP/1.1 200 OK**
  >
  >*start-line = **request-line** / status-line*
  >
  >>request-line
  >>
  >>method SP request-target SP HTTP-version CRLF
  >>
  >>GET 	search?q=hello&hl=ko 	HTTP/1.1
  >>
  >>(SP는 공백, CRLF는 줄바꿈) 

- ##### 응답 메세지

  >HTTP/1.1 200 OK
  >
  >*start-line = request-line / **status-line***
  >
  >>status-line
  >>
  >>HTTP-version SP status-code SP reason-phrase CRLF
  >>
  >>HTTP/1.1	200	OK
  >
  >> HTTP status-code 
  >>
  >> 200: 성공
  >>
  >> 400~ : 클라이언트 요청 오류
  >>
  >> 500~ : 서버 내부 오류



### HTTP 헤더

- HTTP 전송에 필요한 부가정보를 담고 있다.

  메세지 바디 내용, 압축, 클라이언트 정보, 캐시 관리 정보, ...

- header-field = field-name":" OWS field-value OWS (OWS는 띄어쓰기 허용)

>Host: www.google.com
>
>Content-Type: text/html;charset=UTF-8
>
>Content-Length: 3411
>
>...



### HTTP message body

- 실제 데이터 내용
- byte로 표현할 수 있는 모든 데이터 전송 가능
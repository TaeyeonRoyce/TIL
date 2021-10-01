# URL의 입력

네트워크의 시작점은 사용자가 브라우저에서 URL을 입력하는 시점이다.

우선 URL에 대해 알아보자. URL(Uniformed Resource Locator)은 유일한 자원을 가리키는 주소로서, 그 종류가 흔히 아는 `http:` 뿐만 아니라 `ftp:`, `mailto:`등이 있다. 다양한 이유는 각 기능에 따라 엑세스를 할 필요가 있기 때문이다.

- `http:` => 웹 서버에 엑세스하는 경우 //(HTTP protocol)
- `ftp:` => 파일을 업/다운로드 하는 기능 //(FTP protocol)
- `mailto:` => 메일의 기능이 필요한 경우

 `URL`을 받는 브라우저는 웹 뿐만아닌 복합적인 클라이언트라는 것을 알 수 있다.



### URL의 요소

`http:` `//` `www.example.co.kr` `/dir1/index.html`

- `http:` 프로토콜

- `//` : 프로토콜과 서버의 이름 분리

- `www.example.co.kr` : 웹 서버명

-  `/dir1/index.html` : 데이터 파일의 경로 (파일명 생략시 `index.html`이나 `default.html`로 엑세스)

이러한 요소를 갖춘 URL이 입력되면 브라우저가 URL을 해독하여 엑세스할 위치를 알아낸 뒤, HTTP request message를 만든다. 

> *HTTP프로토콜은 `URI`를 `Method`하겠다는 클라이언트의 리퀘스트와 웹서버의 응답메세지를 주고 받을때 요구되는 규칙*

### HTTP Request Message

브라우저가 만든 리퀘스트 메시지를 살펴보자

- 구조

  - 리퀘스트 라인: `<method> <URI> <HTTP version>`
  - 메시지 헤더: `<field name>:<field value>`
  - 메시지 본문: `<message>`

  

  리퀘스트 라인에서는 `method` `URI` `HTTP version`이 표기된다.

  메시지 헤더에서는 부가적인 정보를 쓴다. 헤더 필드의 종류로는 ,`Date` `Authorization` `User-Agent` `Server` `Location` `Content-Length`등이 있다.

  공백 행을 하나 넣고,

  메시지 본문이 표기된다. 메시지 본문은 메시지의 실제 내용이다. `Get` 메소드를 사용하면 아무것도 없겠지만(리퀘스트 메시지니깐), 내가 `HTML`로 만든 파일을 실행하면 `HTML`이 메시지 본문에 들어간다.

리퀘스트 메시지에는 하나의 URI만 들어가기 때문에, 영상이나 사진의 요구되는 경우에는 같은 페이지내에 존재하더라도 각각의 리퀘스트 메시지를 웹 서버에 보내야 한다.

### 응답 메시지

간단하게만 살펴보자.

리퀘스트에 응답으로 기본적인 포맷은 비슷하지만 첫 번째 행이 다르다. `method`나 `URI` 대신  실행 결과를 나타내는 `Status Code`가 들어있다. 자주 접하는 404 페이지의 경우 `클라이언트측의 오류`를 나타내는 코드라고 한다.
















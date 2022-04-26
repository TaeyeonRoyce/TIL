# Restful

Web관련된 자료들을 찾다보면, 심심치 않게 Rest api, Restful 설계 등 REST라는 단어를 자주 접하게 된다.

얼핏 아는 듯한 REST에 대해서 정리해보자

## REST

***Re**presentational **S**tate **T**ransfer* 

**소프트웨어 아키텍처 중 하나.**
**웹**에서 데이터를 전송하고 처리하는 방법을 정의한 인터페이스

"데이터를 전송하고 처리" 한다고 했는데, REST는 처리 방식이 URL을 통해 정의된다는 특징이 있다. REST는 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한으로 활용 가능하다. 

HTTP URI를 통해 Resource를 명시하고, HTTP Method을 통해 해당 자원의 CRUD Operation을 적용한다.

> CRUD Operation : 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create, Read, Update, Delete

이 때, REST는 자원 기반의 구조(ROA, Resource Oriented Architecture)
-> Resource를 중심으로 HTTP Method를 통해 어떻게 처리할지 명시하는 것이다. 모든 자원에 고유한 ID인 HTTP URI를 부여함

예를 들어, User를 조회 한다고 하면 `user`, `Read`라는 처리 명세를 user를 기준으로 한다는 것.
❌ URI :  `/get/user-info` , Method : `GET`
✅ URI :  `/user-info`         , Method : `GET`

### 정리,

- HTTP URI를 통해 각각의 자원(Resource)을 명시

- HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원의 CRUD Operation 적용



## REST 특징

- ### Client-Server 구조

  `Client` : 자원을 요청하는 쪽
  `Server` : 해당 자원이 있는 쪽

  Server에서는 API를 제공하고, 제공된 API를 이용해서 로직을 처리하거나 저장한다.

  Client에서는 사용자 인증이나 세션, 로그인 정보등을 직접 관리하고 책임진다.

  **-> 의존성 감소**

  

- ### Stateless

  작업의 진행도나 상태정보를 저장하지 않는다.

  별다른 세션이나 쿠키등에 대해 생각하지 않고, 단순히 요청에 대해서만 처리한다.

  Server에서는 모든 요청들이 독립적인 것으로 인식하고 처리한다.

  **-> 서비스의 자유도 증가 및 구현의 단순화**



- ### Uniform Interface

  URI로 지정한 Resource에 대한 조작은 통일되고 한정적인 아키텍쳐 스타일로 존재한다.

  **-> HTTP를 따르는 모든 플랫폼에서 사용 가능**

  

- ### Cacheable

  HTTP가 가진 캐싱 기능을 적용 할 수 있다.

  

- ### Self-Descriptiveness (자체 표현 구조)

  REST API의 메세지만 보고도 쉽게 이해할 수 있도록 자체 표현이 가능한 구조로 되어있어야 한다.



- ### Layered System

  계층이 존재한다. (Client -> Server)

  Client는 Server에게 정해진 URL을 요청하고, Server는  보안, 로드 밸런싱, 암호화, Proxy, Gateway같은 계층을 둘 수 있다.

위와 같은 특징들에 따라 REST의 장단점이 존재한다.

### 장점

- HTTP 표준을 활용하기 때문에 캐싱 이외의 추가적인 기능을 활용 가능하다.
- HTTP 표준 프로토콜을 따르는 플랫폼에서 사용 가능하다.
- Server와 Client의 역할을 명확히 분리한다.
- REST API의 역할과 기능에 대해 쉽고 명확하게 파악할 수 있다.

### 단점

- 표준이 존재하지 않는다.

  -> 규칙일 뿐, 엄격한 표준이 존재하지는 않음

- Resource의 처리 방법이 HTTP Method에 제한되어 있다.

- 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재한다.
  (PUT, DELETE를 사용하지 못함)

> 위와 같은 장단점들이 존재하지만, REST는 현재 가장 널리 사용되는 아키텍쳐임에는 틀림없다.
> 다양한 클라이언트(모바일, PC, TV, 태블릿 등등) 플랫폼들이 등장하고, 새롭게 출시되고 업데이트 되는 여러 브라우저들에 대하여 Server는 모두 통신이 가능해야 한다. 이러한 멀티 플랫폼을 지원할 수 있는 Server를 위해 REST가 필요하게 된 것 같다,



## REST API

**REST 기반으로 구현된 서비스 API**

> API?
> **A**pplication **P**rogramming **I**nterface
>
> 컴퓨터 프로그램간의 상호작용(데이터나 기능을 제공, 교환)을 위한 인터페이스

### REST API 디자인 가이드

`핵심 가이드` : URI는 Resource, 해당 Resource에 대한 행위는 HTTP Method

`세부`

1. URI는 Resource만 표현해야 한다.

   >Resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.

2. Resource에 대한 행위는 HTTP Method로 표현해야 한다.

   >URI에 HTTP Method가 들어가면 안됨
   >
   >URI에 행위에 대한 표현이 들어가면 안됨
   >
   >URI경로 중, 변하는 부분은 유일값(ex : id)으로 대체

   ```sql
   GET /get/user/3 -- X
   GET /user/3 -- O
   ```

3. 소문자 사용

   소문자 사용

   > 대소문자에 따라 다른 자원으로 인식되기 때문

4. 공백 대신 하이픈`- `을 사용 (언더바 `_` 는 사용X)

   > Whitespace는 %20이 들어가기 때문에 가독성이 매우 떨어짐

5. 확장자 표기X

   > 확장자 표기는 Resource를 다루는 데 유연성을 떨어뜨린다.
   >
   > Accept Header를 사용하여 해결하도록 함

   ```sql
   http://royce.com/user-thumbnail.jpg -- X
   
   http://royce.com/user-thumbnail -- O
   GET /test HTTP/1.1
   Host: royce.com
   Accept: image/jpg
   ```

   

### REST API 특징

- REST 기반으로 시스템을 분산하여 확장성과 재사용성을 높여 유지 보수 및 운용을 편리하게 할 수 있다.

- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 Client, Server를 구현할 수 있다.

- REST API를 제작하면 델파이 Client 뿐 아니라, Java, C#, Web 등을 이용해 Client를 제작할 수 있다.

  

## RESTful

영어에서 `-ful`은 명사를 형용사로 표현할 때 붙은 접미사이다.

`REST`라는 아키텍쳐를 잘 따르는 `API`가 있다면, `RESTful API`라 부를 수 있지 않을까...?
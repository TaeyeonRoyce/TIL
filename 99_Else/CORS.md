# CORS

### 교차 출처 리소스 공유(Cross-Origin Resource Sharing)

웹은 사용자의 공격에 너무나 취약하다. 브라우저가 제공해주는 웹브라우저의 개발자 도구만 들어가도 script부터 page source까지 암호화 없이 확인 가능하고, 사용자가 임의로 추가 및 수정도 가능하기 때문이다.

이러한 웹의 취약점 때문에 브라우저에서 제공하는 보안 매커니즘들이 있는데, 그 중 하나가 동일 출처 정책(SOP)이다. 이 정책이 존재하기 때문에 다른 출처에서의 리소스 요청이 불가하다. React와 Spring을 활용하여 API를 호출 할 때, 둘이 사용하는 포트가 달라 API호출이 일반적으로 제한된다.

# SOP

### 동일 출처 정책 (Single Origin Policy)

> 두 URL의 프로토콜, 포트(명시한 경우), 호스트가 모두 같아야 동일한 출처라고 말한다.

한 출처에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하면서 잠재적으로 해로울 수 있는 문서를 분리할 수 있고, 이를 통해 공격받는 경로를 줄일 수 있다고 한다.

#### 다음과 같은 예시는 출처(Origin)이 다른 경우이다.

- ex.1) 포트가 다른 경우

| URL                     | 프로토콜 | 호스트    | 포트 |
| ----------------------- | -------- | --------- | ---- |
| `http://localhost:3000` | http     | localhost | 3000 |
| `http://localhost:8080` | http     | localhost | 8080 |

- ex.2) 호스트가 다른 경우

| URL                     | 프로토콜 | 호스트 | 포트 |
| ----------------------- | -------- | ------ | ---- |
| `http://hostA:8080.com` | http     | hostA  | 8080 |
| `http://hostB:8080.com` | http     | hostB  | 8080 |

#### 출처(Origin)이 동일한 경우

| URL                           | 프로토콜 | 호스트    | 포트 |
| ----------------------------- | -------- | --------- | ---- |
| `http://localhost:8080`       | http     | localhost | 8080 |
| `http://localhost:8080/hello` | http     | localhost | 8080 |



# Prefilght Request

브라우저에게 리소스를 받아오라는 명령을 내리면(ex API호출), 브라우저가 해당 서버에게 Prefilght Request를 먼저 보낸다. 서버에서는 이 Prefilght Request에 대한 응답으로 어떤 것들을 허용하고 금지하고 있는지에 대한 정보를 담아 응답한다.

브라우저는 응답받은 메세지를 통해 브라우저가 요청받은 명령이 가능한지 판단하고, 가능하다면 진짜 요청을 보내게 된다.

>`client` --(request)-> `browser`,
>
>`browser` --(preflight request)-> `server`, `server` --(response)-> `browser`
>
>`browser` --(request)-> `server`

Preflight request의 응답에 포함되어 있는  Access-Control-Allow-Origing헤더를 통해 CORS인지 아닌지를 판단한다.
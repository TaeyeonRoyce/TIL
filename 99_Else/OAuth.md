# OAuth

인증을 위한 Open Standard Protocol

> 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용 되는, 접근 위임을 위한 개방형 표준

어느 App에서 Google로 로그인이라는 기능이 있을 때, 이 App은 사용자 인증을 위해 Google의 사용자 인증 방식을 사용한다. 이 때,  OAuth를 바탕으로 Google로 부터 특정 자원에 접근하거나 사용할 수 있는 권한을 인가받게 된다.

## OAuth 원리와 과정

OAuth 동작에 관여하는 참여자는 크게 세 가지로 구분할 수 있다.

- Resource Server : Client가 제어하고자 하는 자원을 보유하고 있는 서버입니다.
  - Facebook, Google, Twitter 등이 이에 속합니다.
- Resource Owner : 자원의 소유자입니다.
  - Client가 제공하는 서비스를 통해 로그인하는 실제 유저
- Client : Resoure Server에 접속해서 정보를 가져오고자 하는 클라이언트(웹 어플리케이션)

ex) A가 웹 서비스B 에 Google 아이디로 로그인 하는 경우

- Resource Server : Google
- Resource Owner : A
- Client : 웹 서비스 B

## OAuth Flow

1. Client 등록

   Client는 Resource Server를 이용하기 위해 Client의 서비스를 등록하여 승인을 받아야 한다.

   등록을 하고 나면, Client ID, Client Secret, Authorized redirect URL 정보를 받는다.

   Resource Owner가 Resource Server를 통해 인증을 하고 난 뒤, Authorized redirect URL로 리다이렉트 시키며 어떤 코드를 함께 전송한다. 이 Code와 ClientID, Client Secret을 Resource Server에 보내 Access Token을 받는다.

2. Resource Owner의 승인

   Resource Server에 등록하면서 받은 ClientID와 RedirectUrl을 이용하여 Resource Owner가 로그인 버튼을 클릭하면 이동하여 Resource를 요청할 주소를 명시한다.

   `GET <https://github.com/login/oauth/authorize?client_id={client_id}&redirect_uri={redirect_uri}?scope={scope}`>

   위 주소에서 Resource Owner가 로그인을 완료하면, Resource Server에서는 Query String으로 넘어온 파라미터들을 보며 Client를 검사한다.

   - 파라미터로 전달된 Client ID와 동일한 ID 값이 존재하는지 확인
   - 해당 Client ID에 해당하는 Redirect URL이 파라미터로 전달된 Redirect URL과 같은지 확인

   검사가 완료되면, Resource Ownder의 승인을 받고, Client(not Resource Owner)를 Rediret URL로 리다이렉트 시킨다.

3. Resource Server의 승인

   Client를 Redirect 시킬 때, Resource Server는 Client가 자신의 자원을 사용할 수 있는 Access Token을 발급하기 전에, 임시 암호인 Authorization Code를 함께 발급한다.

   `localhost:8080/afterlogin?state=afdafadf&code=8492ab928100bc7s`

   Client (OAuthController.java)

   ```java
   @GetMapping("/afterlogin")
   public ResponseEntity<String> afterlogin(@RequestParam String code) {
       if (Objects.isNull(code)) {
           throw new IllegalStateException("Github 로그인이 실패했습니다.");
       }
       RestTemplate restTemplate = new RestTemplate();
       OauthDto oauthDto = new OauthDto();
       oauthDto.setCode(code);
       oauthDto.setClient_ID("7e930566ecaa306c71b5");
       oauthDto.setClient_secret("client secret 입력");
       String accessToken = restTemplate.postForObject("<https://github.com/login/oauth/access_token>", oauthDto, String.class);
       return ResponseEntity.ok(accessToken);
   }
   ```

   임시 암호인 Code와 Client등록시 등록해 둔 ClientID와 Client Secret을 함께 Resource Server로 전송한다. Resource Server에서는 전송된 정보들을 확인하여 Access Token을 발급한다.

이 Access Token을 포함하여 Resource Server에서 제공하는 API에 요청을 보내면, Scope에서 설정한 Resource Owner의 Resources들을 받을 수 있다.

Refresh Token

Access Token은 만료 기간이 있으며, 만료된 Access Token으로 API를 요청하면 401 에러가 발생함.

보통 Resource Server는 Access Token을 발급할 때 Refresh Token을 함께 발급하고, Client가 Access Token을 포함해서 요청을 보냈을 때, 만료되어 401 에러가 발생하면, Client는 보관 중이던 Refresh Token을 보내 새로운 Access Token을 발급받게 된다.

from : [OAuth 개념 및 동작 방식 이해하기](https://tecoble.techcourse.co.kr/post/2021-07-10-understanding-oauth/)
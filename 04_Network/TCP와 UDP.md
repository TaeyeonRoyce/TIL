# TCP와 UDP

`TCP`와 `UDP`는 **TCP/IP의 전송계층**에서 사용되는 프로토콜이다. 

>TCP/IP란?
>
>인터넷 표준 Protocol로서 컴퓨터 간 데이터를 통신할 때 발생 될 수 있는 오류를 방지하기 위함.
>
>보통, TCP는 데이터의 정확성을 검증하고 IP는 패킷의 목적지까지 전송하는 역할을 수행한다.



TCP/IP 계층 중 전송계층에서는 데이터의 전달을 담당하는데, 이때 데이터를 보내기 위해 TCP 혹은 UDP Protocol이 사용될 수 있다.

>TCP/IP 계층과 OSI 7계층은 다르다..!

TCP와 UDP를 사용하는 판단 기준은, 그 Protocol만의 특징에 의해 결정되어 사용할 수 있으므로 각각에 대해 살펴보도록 하자.



## TCP

***T**ransmission **C**ontrol **P**rotocol* => 전송 제어 프로토콜

TCP는 일반적으로 IP와 함께 사용되며 다음과 같은 역할을 수행한다.

- IP : 데이터의 배달을 수행
- TCP : 데이터(패킷)을 추적 및 관리하는 역할

TCP의 가장 큰 특징은 통신의 대상이나 상태가 *신뢰 가능한 상태*임을 제공 해준다는 것이다. 전송 과정에서의 누락이나 손실, 순서의 변경등 발생하는 이슈들을 관리 해준다. 추가로 다음과 같은 특징들이 있다.

- `흐름 제어` : 데이터의 처리 속도를 조절하여 버퍼 오버플로우를 방지한다.
- `혼잡 제어` : 네트워크 내의 패킷 수를 조절하여 혼잡도를 관리한다.
- `신뢰가능한 전송` : ACK제어비트를 활용하여 정상적인 송수신이 이루어 지는지 확인한다.
- `연결 지향` : 연결형 서비스로 가상 회선 방식을 제공한다.
  => 송신측과 수신측을 연결하여 패킷을 전송하기 위한 논리적 경로를 배정한다는 뜻이다.

수신측과 송신측을 연결할 땐 3-way handshaking 방식이, 연결을 종료할 땐 4-way handshaking방식이 사용된다

### 3-way, 4-way handshaking (연결과 해제)

![img](https://nesoy.github.io/assets/posts/20181010/2.png)

1. 먼저 open()을 실행한 클라이언트가 `SYN`을 보내고 `SYN_SENT` 상태로 대기한다.
2. 서버는 `SYN_RCVD` 상태로 바꾸고 `SYN`과 응답 `ACK`를 보낸다.
3. `SYN`과 응답 `ACK`을 받은 클라이언트는 `ESTABLISHED` 상태로 변경하고 서버에게 응답 `ACK`를 보낸다.
4. 응답 `ACK`를 받은 서버는 `ESTABLISHED` 상태로 변경한다.

> `ACK` : 수신측에서 사용하며 패킷이 정상적으로 도착했는지를 나타내는 제어 비트의 캐릭터
>
> `SYN` : 양측에서 최초로 연결을 하는 경우 사용되는 제어 비트의 캐릭터

### 4-way handshaking

1. 먼저 close()를 실행한 클라이언트가 FIN을 보내고 `FIN_WAIT1` 상태로 대기한다.
2. 서버는 `CLOSE_WAIT`으로 바꾸고 응답 ACK를 전달한다. 동시에 해당 포트에 연결되어 있는 어플리케이션에게 close()를 요청한다.
3. ACK를 받은 클라이언트는 상태를 `FIN_WAIT2`로 변경한다.
4. close() 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 `FIN`을 클라이언트에 보내 `LAST_ACK` 상태로 바꾼다.
5. FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 `TIME_WAIT`으로 상태를 바꾼다. `TIME_WAIT`에서 일정 시간이 지나면 `CLOSED`된다. ACK를 받은 서버도 포트를 `CLOSED`로 닫는다.

from : https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4



연결과 전송에 대한 확인하는 절차가 존재한다. 이러한 방식 때문에 신뢰성을 보장한다고 말하며 1:1로 연결된다는 특징 또한 존재한다.

**속도 보다는 신뢰성에서 이점을 얻을 수 있다**



## UDP

***U**ser **D**atagram **P**rotocol* => (사용자 데이터그램 프로토콜)

데이터를 데이터그램 단위로 처리한다는 특징이 있다. (전송중인 데이터들 간의 패킷은 서로 독립적이다)

`TCP`와 달리 비연결형 프로토콜로, 특정한 논리경로 없이 모든 패킷들이 독립적으로 수신측에 전송한다는 특징이 있다.

> 예를 들어, 패킷1, 2, 3이 존재한다면
>
> TCP를 사용한 전송은 패킷1 -> 패킷 2 -> 패킷 3의 순서를 보장받는 경로를 통해 수신측에 도착한다면
>
> UDP는 보장없이 도착할 수 있다. (패킷 1, 2, 3이 서로 독립적임)

`UDP`는 연결과 해제의 과정이 존재하지 않다. 헤더에 존재하는 `CheckSum` 필드를 통해 최소한의 오류만을 검증하고 전송이 이루어 진다.

`TCP`에 비하면 전송의 신뢰도나 안전성은 떨어지지만 빠르다는 특징이 있다. 또, 비연결형이다 보니 `1 : 1` 뿐만 아니라 `1 : N`, `N : M`등으로 연결 될 수 있다.

실시간 스트리밍이나 채팅 같은 연속성이 중요한 서비스에 사용된다고 한다.



## TCP vs UDP

마지막으로 두 Protocol의 차이에 대해 정리해보면,

| 프로토콜 종류  |        TCP         |            UDP            |
| :------------: | :----------------: | :-----------------------: |
|   연결 방식    |   연결형 서비스    |      비연결형 서비스      |
| 패킷 교환 방식 |   가상 회선 방식   |      데이터그램 방식      |
|   전송 순서    |   전송 순서 보장   | 전송 순서가 바뀔 수 있음  |
| 수신 여부 확인 | 수신 여부를 확인함 | 수신 여부를 확인하지 않음 |
|   통신 방식    |      1:1 통신      | 1 : 1 or 1 : N, N : M 등  |
|     신뢰성     |        높다        |           낮다            |
|      속도      |       느리다       |          빠르다           |


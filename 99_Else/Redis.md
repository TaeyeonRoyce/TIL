Redis를 많이 사용한다고는 알고 있었지만 크게 다룰 일이 없었어서 적당하게 특징에 대해서만 인지하고 있었다.
하지만, 프로젝트에서 실시간으로 데이터를 조회해야 하는 기능이 생겨 속도에 대한 고민이 생겼고, Redis를 도입하여 해결해보자고 결정하였고, 이렇게 정리를 해보고자 한다.

# [Redis](https://redis.io/docs/)
**RE**mote **DI**ctionary **S**erver
> _Dictionary란, Key-value 형태의 값을 저장할 수 있는 자료구조_

Redis에 대해 직역하자면, 원격에서 Key-Value 형태의 값을 저장할 수 있는 서버이다.
추가로, Redis는 메모리 기반으로 동작하여 DB에 직접 접근하는 방식에 비해 상당히 빠른 성능을 기대할 수 있다.



그렇다면, 왜 DB에 직접 접근하는 것보다 속도를 기대할 수 있을까?
메모리 계층 구조와 관련이 있는데, 아래 사진을 참고해보자

### 메모리 계층 구조
![](https://velog.velcdn.com/images/roycewon/post/e90a071e-3e61-48f6-b3b9-e18b9ef34b06/image.png)

주 메모리 (RAM)이 Persistent Storage(Disk)보다 Latency가 낮고 용량은 적다는 특징이 있다. 속도의 차이는 소재나 용량, 설계 등에 의해 발생하는데 간단하게 사진으로 이해하고 넘어가자.
[궁금하면](https://quasarzone.com/bbs/qf_cmr/views/343277)

결론적으론, 메모리 계층 구조를 이해하고 나면 메모리에 접근하는 것이 속도가 더 빠르다는 것을 알 수 있다. 추가로 메모리는 휘발성이라는 점도 알고 넘어가자.


# Redis 특징
### key-value
Redis는 key-value 데이터 저장소이다. Java에서의 Map과 유사한데, Map자료구조의 장점과 동일하다. 데이터를 읽고 쓰는 속도가 빠르다는 것이다.


### 여러 데이터타입 제공
![](https://velog.velcdn.com/images/roycewon/post/062d9400-e941-4925-ad7f-ab011685bffa/image.png)


key-value의 value 타입에 대해 다양하게 제공이 된다. 간단하게 String부터 Collection인 List, Set, 심지어 Sorted set 까지 제공하여 상황에 맞게 데이터를 저장할 수 있다.

> 1. #### String
> 일반적인 문자열로 최대 512MB 길이까지 지원한다. Text 문자열뿐만 아니라 Integer와 같은 숫자나 JPEG 같은 Binary File까지 저장할 수 있다고 한다.

>2. #### Set
>Redis에서 제공하는 `Set`은 `Set<String>`이다. (문자열 Set) 일반적인 `Set`자료구조와 동일하게 중복은 저장이 되지 않는다.

>3. #### Sorted Set
>`Set`에 가중치가 존재하여 오름차순으로 내부가 정렬되어 있다.

> 4. #### Hashes
> value 내에 `{field/string : value}` 쌍으로 이루어진 테이블을 저장하는 구조.

> 5. #### List
> `List<String>` (문자열 List) 이며 `List` 자료구조보단 `Deque`와 비슷하다. List의 앞, 뒤에서 push와 pop 연산을 제공한다

다양한 데이터 타입(Collection) 제공은 개발의 편의성과 난이도에서 이점을 기대할 수 있다.

간단하게, 캐시나 세션 값(`String`)을 저장하거나 실시간 랭킹을 구해야 하는 기능에선 `Sorted Set`을 사용하면 굉장히 편리하다.

### Single Threaded
Redis는 싱글쓰레드로 동작한다. 당연하게도, Atomic하여 Race condition이 발생하지 않는다. 이러한 이유가 여러 서버에서 같은 데이터를 공유하고 보고싶을 때  Redis를 많이 사용하는 이유가 될 수 있을 것 같다. 
(인증 토큰 저장, API limit 등등)

하지만 이 특징은 성능 부분에 대해서 신경을 써야하는데, 시간 복잡도 $O(n)$, 혹은 그 이상 스케일의 명령은 지양해야 한다. 해당 연산에 붙잡혀 다른 프로세스가 처리되지 않고 쌓이기 때문이다.
![](https://velog.velcdn.com/images/roycewon/post/4b47235b-b5ae-49a6-8c83-c8b691aeba2b/image.png)



### Persistence(Disk에 저장이 가능하면서 생긴 영속성)
Redis는 메모리에서 동작하여 데이터 저장소로 사용해도 괜찮을까 고민할 수 있다. 다행히도, redis는 disk에 저장하여 영속성을 제공해준다. redis의 데이터를 disk에 저장하는 방식은 snapshot, AOF 방식이 있다고 한다.

> RDB : 특정 시점의 데이터를 DISK에 옮겨담는 방식 (Redis fork로 수행)

> AOF : Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록한 뒤, 서버가 재시작할 때 로그를 기반으로 데이터를 복구

>[The most important thing to understand is the different trade-offs between the RDB and AOF persistence.](https://redis.io/docs/manual/persistence/)

<br>

# Cache로 사용
다시 처음으로 돌아가서, Redis를 사용하기로 한 이유에 집중 해보자.
> #### "실시간으로 데이터를 조회"

클릭 한 번의 API 호출에 비해 굉장히 많은 요청 혹은 DB접근이 발생할 것으로 예상이 되었다. In-Memory로 동작하는 Redis의 사용 이뉴는 DB 접근을 줄여 속도 향상을 기대한다는 것이다.
그럼 어떻게..??

DB에 대한 Cache처럼 사용하면 된다. 간단하게, Cache는 요청된 결과를 미리 저장해두었다가 다시 요청할 때, 빨리 제공하기 위해 사용되는 개념이다. 이를 차용해서, DB에 대한 정보를 Redis에 캐싱해 두는 것이다. DB보다는 용량(Capacity)는 적다는 점도 고려하자.

이렇게 캐싱하는 것도 두가지 전략이 있다고 한다.
### 1. Look aside cache (Lazy Loading)

1.  웹 서버는 클라이언트 요청을 받아서, 데이터가 존재하는지 캐시를 **먼저** 확인
2. Cache에 데이터가 있으면 그걸 꺼내주는데, 만약 없으면
3. DB에서 읽어서 -> 먼저 캐시에 저장한다음 클라이언트에게 데이터를 돌려줌

일반적으로 사용됨

### 2. Write Back
1. Data를 Cache에 저장
2. Cache에 있는 Data를 일정 기간동안 Check
3. 모여있는 Data를 DB에 저장
4. Cache에 있는 Data 삭제

데이터를 캐시에 전부 저장해놓았다가 특정 시점마다 한번식 캐시 내 데이터를 DB에 insert하는 방법이다.

# 정리
Redis에 대한 일반적인 개념들에 대해 살펴보았다. 특징들을 파악해 보니, Redis를 통해 해결할 수 있는 문제가 많을 것 같다. (속도, 공유, 동시성 관련) 더 나아가 Redis를 여러개 운영하거나 복제하여 문제에 대비하는 등, 아키텍쳐에 대한 정보도 많더라.

직접 사용해보면서 익히면 좋을 것 같다.

> 참고
> 공식 문서 : https://redis.io/docs/
> @swhan9404 : [Redis 란 무엇인가?](https://velog.io/@swhan9404/Redis-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)
> @hyeondev : [Redis 란 무엇일까?](https://velog.io/@hyeondev/Redis-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C#redis-cluster)
> [우아한 레디스](https://www.youtube.com/watch?v=mPB2CZiAkKM)
> [[10분 테코톡] 🤔디디의 Redis](https://www.youtube.com/watch?v=Gimv7hroM8A)
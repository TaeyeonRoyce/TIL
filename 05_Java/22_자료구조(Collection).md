# Collection Framework

`Java`에서도 여러 자료구조들을 라이브러리를 통해 제공하고 있다.

`ArrayList`,`LinkedList`,`Stack`,`Queue`,`Map`,`Set` ... 등등



### List 인터페이스

상속관계 : *Collection 인터페이스* -> *List 인터페이스* -> `ArrayList, LinkedList, ...`

흔히 쓰이는 `ArrayList`나 `LinkedList` 와 다르지 않다.

- 중복을 허용하고, 순차적인 데이터의 접근이나 변경에 유리하다.

```java
ArrayList list1 = new ArrayList();
LinkedList list2 = new LinkedList();
```

#### ArrayList, LinkList 메서드

- `.add(Object )` : 맨 뒤에 추가
- `.contains(Objcet )` : 객체가 포함되어 있는지 확인
- .`size()`: 저장된 객체의 개수 반환
- `.get(int )` : 지정된 위치에 저장되어있는 객체 반환
- `.set(int , Object )`: 지정된 위치에 주어진 객체 저장
- `.sort()` : 정렬

#### Stack

- `.pop()` , `.push()` : 꺼내고, 넣기

#### Deque

- `.offerFirst()`, `.offerLast()` :  앞에서부터, 뒤에서부터 추가
- `.pollFirst()`, `.pollLast()`: 앞에서부터, 뒤에서부터 삭제



## Map 인터페이스

- 데이터를 키와 값의 쌍으로 저장
  (딕셔너리)
- 키와 값의 한 쌍을 `entry`라고 한다

#### HashMap을 통해 구현 가능

- 데이터 검색에서 유리함.
- 키는 유일해야하며,  값은 중복이 허용된다

#### 메서드

- `.put(Object key, Object value)`: 키와 값을 가진 entry 저장
- .`remove(Object key)`: 해당 키를 가진 entry 삭제
- `.get(Object key)`: 해당 키를 가진 entry의 값 반환
- `.containsKey(Object key)`, `containsValue(Object value)` : 키, 값 조회후 boolean 반환
-  `.size()`,`.clear()`






# Null merge operator

(병합 연산자)



>*`null`은 `boolean`연산에서는 거짓으로 취급한다.*

## `??`

병합 연산자(`??`, `||`)을 활용하여 짧은 문법으로 확정되어있는 변수를 찾을 수 있다.

`a ?? b`의 결과

- `a`가 `null`도 아니고 `undefined`도 아니면 `a`
- 그 외의 경우는 `b`

예제

```js
let firstName = null;
let lastName = null;
let nickName = "바이올렛";
// null이나 undefined가 아닌 첫 번째 피연산자
alert(firstName ?? lastName ?? nickName ?? "익명의 사용자"); // 바이올렛
```

이렇게 병합연산자를 연속으로 사용하면, 사용자 입력의 부재를 간단하게 처리할 수 있다.

### `||`

`||`는 `OR`연산자지만 병합연산자`??`처럼 사용할 수 있다.

`a || b`의 결과

- `a or b`가 참이면 `a`, 아니면 `b`

```js
let firstName = null;
let lastName = null;
let nickName = "바이올렛";
// null이나 undefined가 아닌 첫 번째 피연산자
alert(firstName || lastName || nickName || "익명의 사용자"); // 바이올렛
```

같은 결과를 기대할 수 있지만 조금 다른점이 있다.

## 차이점

- `||`는 첫 번째 *truthy* 값을 반환합니다.
- `??`는 첫 번째 *정의된(defined)* 값을 반환합니다.

이런 차이점은 `null`,`undefined` 말고 추가적인 `falsy`한 값을 비교할때 생기는데, 아래 예제를 보자

```js
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

`height || 100`는 `height`에 0이란 값이 존재하지만, 0이 `falsy`한 값이라 `truthy`한 값인 100을 반환한다.

### falsy

`false`말고 `false`스러운 값들을 얘기하는데, js에선 다음과 같은 값들을 `falsy`하다고 한다

```js
false
null
undefined
0
-0
0n 
NaN 
""
-MDN
```


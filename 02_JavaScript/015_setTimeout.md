# Scheduling a call

호출 스케줄링

일정 시간이 지난 후에 원하는 함수를 실행하기

### setTimeout

##### 구문

```js
let setTimeFunc = setTimeout(func, [delay], [arg1], [arg2], ...)
```

`func` : 실행하고자 하는 함수

`delay` : 실행 대기 시간, 단위는 millisecond(1/1000sec), `optional`,기본값은 0

`arg1,arg2,...`: 타이머가 만료되고 `func`에 전달되는 추가적인 매개변수. `optional`

```js
function sayHi() {
  console.log('Hello World!');
}
function sayHiTo(user){
  console.log('Hello' + user);
}

setTimeout(sayHi, 1000); //1초뒤 sayHi 실행
setTimeout(()=>console.log('Hello World!'), 2000); //2초뒤 ()=> 실행
setTimeout(sayHiTo, 3000, 'Royce'); // 3초뒤 sayHiTo('Royce') 실행
```



#### Timer

`setTimeout`가 호출되면 타이머를 실행하고 `timeoutID`를 반환하는데, 이를 활용해서 실행된 timer를 멈출 수 있다.

```js
let timerId = setTimeout(...); 
clearTimeout(timerId);
```

`clearTimeout`은 반환된 `timeoutID` 를 멈추기만 하고 0으로 초기화하는 것은 아니다.



### setInterval

`setTimeout`과 문법은 비슷하지만, `delay`가 `interval`로 바뀐다.

```js
// 2초 간격으로 메시지를 보여줌
let timerId = setInterval(() => console.log('째깍'), 2000);

// 5초 후에 정지
setTimeout(() => { clearInterval(timerId); console.log('정지'); }, 5000);
```

### 중첩 setTimeout

위 코드를 다르게 표현하면,

```js
let timerId = setTimeout(function tick() {
  console.log('째깍');
  timerId = setTimeout(tick, 2000);
}, 2000);
setTimeout(() => clearTimeout(timerId), 5000);
```

이렇게 `setTimeout`을 재귀 실행하는 것이 `setInterval`을 사용하는 것보다 유리하다.

우선, 조금더 유연하게 시간을 조절할 수 있다는 점이 있다. 상황에 따라 재귀로 들어가는 `delay`간격을 조절 할 수 있기 때문이다.

또 간격에 대한 정확한 표현이 가능하다.

`setInterval`에서의 `interval`은 내부함수를 실행하는 시간을 포함해서 `interval`이 되고, `setTimeout`의 재귀 실행에서는 내부함수를 실행 한 뒤 `delay`가 시작되기 때문에 명시한 간격을 정확히 실행된다.

#### 지연

내가 설정한 간격이나 지연시간과 다르게 호출될 수 도 있다.

 그 이유로는 

- CPU가 과부하 상태인 경우
- 브라우저 탭이 백그라운드 모드인 경우
- 노트북이 배터리에 의존해서 구동 중인 경우

등이 있으니 명시한 시간이 정확히 구현되지 않을 수도 있다는 점을 알아두자.




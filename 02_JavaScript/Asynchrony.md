# 비동기(asynchrony)



## 비동기란?

특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 자바스크립트의 특성을 의미한다.

자바스크립트는 call stack이 하나로 동작하기 때문에 마치 싱글스레드 기반의 언어처럼 작동하는데, 멀티스레드와 비슷하게 부드러운 소프트웨어를 만들기 위해선 비동기적인 프로그래밍이 필요하다.

- Ajax 호출을 비롯한 네트워크 요청
- 파일을 읽고 쓰는 등의 파일시스템 작업
- 의도적으로 시간 지연을 사용하는 기능(알람 등)
- 사용자 입력 후 작동하는 기능

의 경우 사용한다.

예시로,

``` js
function getData() {
	var tableData;
	$.get('https://domain.com/products/1', function(response) {
		tableData = response;
	});
	return tableData;
}

console.log(getData()); // undefined

//출처 Captain Pangyo님의 깃허브
```

위 예시에서 ajax통신을 할 때 데이터를 하나 요청하는 코드인데, `console.log(getData())` 의 결과가 `undefined` 인데, 그 이유는 `get`으로 데이터를 완전히 받아오기도 전에 `return tableData`가 실행되었기 때문이다.



```js
 function getData(callbackFunc) {
	$.get('https://domain.com/products/1', function(response) {
		callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
	});
}

getData(function(tableData) {
	console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});

//출처 Captain Pangyo님의 깃허브
```

위 코드에선 콜백 함수를 사용하여 비동기적 프로그래밍을 구현했다.

`get`이라는 로직을 마치고 나서 `callbackFunc(response)`가 이루어지므로 `getData()`에 데이터가 전달된다.



이처럼 비동기 프로그래밍을 통해 좀 더 부드럽고 효율적인 구현이 가능하다.

비동기 처리 방식으로는 `Callback`, `Promise` `Async / Await` 등의 방식들이 있다.
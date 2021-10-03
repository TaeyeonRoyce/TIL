# Promise

## Promise란?

​	비동기 작업의 상태를 나타내는 객체로 Callback 지옥의 단점을 극복하기 위해 사용. 함수에 Callback을 첨부하는 방식의 객체이다. 비동기 작업의 상태로는 3가지가 있으며, pending, fulfilled, rejected가 있다.

예시를 보며 이해해보자

이전 `Asynchrony.md`에서 보았던 예시

```js
function getData(callbackFunc) {
  $.get('url 주소/products/1', function(response) {
    callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
  });
}

getData(function(tableData) {
  console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

==> Promise

```Js
function getData(callback) {
  // new Promise() 추가
  return new Promise(function(resolve, reject) {
    $.get('url 주소/products/1', function(response) {
      // 데이터를 받으면 resolve() 호출
      resolve(response);
    });
  });
}

// getData()의 실행이 끝나면 호출되는 then()
getData().then(function(tableData) {
  // resolve()의 결과 값이 여기로 전달됨
  console.log(tableData); // $.get()의 reponse 값이 tableData에 전달됨
});
```

`new Promise()` , `reslove()` `then()`

## new Promise()

​	promise는 앞서 말한 3가지 상태를 갖는다.

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
- Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태

`new Promise()` 는 callback함수를 인자로 가지며 선언을 할 수 있고, 그 callback함수는 resolve, reject 라는 인자를 가실 수 있다.

```js
new Promise(function(resolve, reject) {
  // ...
});
```

`new Promise()`를 호출하면 `Pending`상태이고, 

```js
new Promise(function(resolve, reject) {
  resolve();
});
```

`resolve()`가 호출되면 Fullfilled상태가 된다.

```js 
function getData() {
  return new Promise(function(resolve, reject) {
    var data = 100;
    resolve(data);
  });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function(resolvedData) {
  console.log(resolvedData); // 100
});
```

`then()`을 이용하여 처리 결과를 사용할 수 있다.



```js
function getData() {
  return new Promise(function(resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

// reject()의 결과 값 Error를 err에 받음
getData().then().catch(function(err) {
  console.log(err); // Error: Request is failed
});
```

​	`reject()`가 호출되면 Rejected 상태가 된다. 그리고 그 결과값을 `catch`로 받을 수 있다.



## Promise의 장점

callback에서 비동기 작업을 연속적으로 수행하면서 생기는 콜백지옥같은 상황을 피해, `promise chaining`을 통해 모던한 방식으로 접근이 가능하다.

예시

```js
new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(1);
  }, 2000);
})
.then(function(result) {
  console.log(result); // 1
  return result + 10;
})
.then(function(result) {
  console.log(result); // 11
  return result + 20;
})
.then(function(result) {
  console.log(result); // 31
});
```

`resolve()`가 호출되면 Promise가 Pretending 상태에서 Fulfilled 상태로 넘어가기 때문에 첫 번째 `.then()`의 로직으로 넘어간다. 첫 번째 `.then()`에서는 이행된 결과 값 1을 받아서 10을 더한 후 그다음 `.then()` 으로 넘겨준다. 두 번째 `.then()`에서도 마찬가지로 바로 이전 프로미스의 결과 값 11을 받아서 20을 더하고 다음 `.then()`으로 넘겨주고, 마지막 `.then()`에서 최종 결과 값 31을 출력.



## Promise 에러 처리

`catch()`를 활용해서 예외 처리 권장.

```js
// catch()로 오류를 감지하는 코드
function getData() {
  return new Promise(function(resolve, reject) {
    resolve('hi');
  });
}

getData().then(function(result) {
  console.log(result); // hi
  throw new Error("Error in then()");
}).catch(function(err) {
  console.log('then error : ', err); // then error :  Error: Error in then()
});
```




# Null & Undefined



null과 undefined는 '없음'을 나타낼때 사용되지만 그 목적과 위치에 따라 다르게 사용된다.



null 은 값은 값이지만 값으로써 의미없는 특별한 값이 등록되어 있는 것이고, undefined 는 등록이 되어있지 않기 때문에 초기화도 정의되지도 않은 것.

undefined 는 미리 선언된 전역변수이며, null 은 선언,등록을 하는 키워드임

`null` 은 존재하는 변수가 아무런 값도 없을 경우

`undefined`는 그 존재가 없거나, 초기화되지도 않은 경우

```js
// 정의되지 않고 초기화된 적도 없는 foo
foo; //undefined

// 존재하지만 값이나 자료형이 존재하지 않는 foo
var foo = null;
foo; //null  //not undefined
```

 `null`과 `undefined`의 `type`을 보면 좀더 이해하기 쉽다.

```js
typeof null // 'object'
typeof undefined // 'undefined'
```



```js
let foo; // 값을 대입한 적 없음
let bar = undefined; // 값을 대입함
console.log(foo); // undefined
console.log(bar); // undefined (??)
let obj1 = {}; // 속성을 지정하지 않음
let obj2 = {prop: undefined}; // 속성을 지정함
console.log(obj1.prop); // undefined
console.log(obj2.prop); // undefined (??)
```

이렇게 값이 존재하지 않은 경우에도`null`이 아닌 `undefined`값으로 나오는데, 같은 '없음'을 나타내지만 값의 부재를 나타내고 싶은 경우에는 `null`을 사용하는 것을 권장함


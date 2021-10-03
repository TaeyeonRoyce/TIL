# This



### `this`란?

JS에서 함수가 호출될때 전달되는 매개변수 이외에도 `this`가 암묵적으로 전달되는데, 이때 `this`의 값은 함수를 호출한 방법에 의해 결정된다. 다른 언어, `Swift`의 `self`나 `Java`의 `this`와 약간의 다른 점이 바로 이 점이다.

JS에서 함수가 호출되는 방식은

1. 함수 호출
2. 메소드 호출
3. 생성자 함수 호출
4. apply/call/bind 호출

이 있다.

```js
var foo = function () {
  console.dir(this);
};

// 1. 함수 호출
foo(); // window
// window.foo();

// 2. 메소드 호출
var obj = { foo: foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
var instance = new foo(); // instance

// 4. apply/call/bind 호출
var bar = { name: 'bar' };
foo.call(bar);   // bar
foo.apply(bar);  // bar
foo.bind(bar)(); // bar
```



### 함수호출

일반적인 호출 방법으로 최상위 객체로 결정되며 Browser에서는 `window`에 해당한다.

```js
this === window // true
```

이때 `this`는 전역객체인 `window`에 바인딩 된다.

```js
function foo() {
  console.log("foo's this: ",  this);
  function bar() {
    console.log("bar's this: ", this);
  }
  bar();
}
foo();
// 내부함수`bar()`도 window에 바인딩 된다.
```

내부함수에서는 일반 함수, 메소드, 콜백함수 어디에서 선언되었든 관게없이 this는 전역객체를 바인딩한다

### 메소드 호출

함수가 객체의 프로퍼티 값이면 메소드로서 호출되는데, 메소드 내부의 `this`는 해당 메소드를 소유한 객체, 즉 해당 메소드를 호출한 객체에 바인딩된다.

```js
var obj1 = {
  name: 'Lee',
  sayName: function() {
    console.log(this.name);
  }
}

var obj2 = {
  name: 'Kim'
}

obj2.sayName = obj1.sayName;

obj1.sayName();  //Lee
obj2.sayName();  //Kim
```


# Bind

JS에서는 `this`는 `this `를 포함한 함수를 호출한 방법에 의해 결정되기 때문에 `this` 의 값이 의도치 않게 바뀔 수도 있다.

`bind`는 보통 객체 메서드의 `this`를 고정해 어딘가에 넘기고자 할 때 사용합니다

### 객체의 메소드를 호출 할 때

```js
let x = 9;
let module = {
  x: 81,
  getX: function(){
    console.log(this.x);
  }
};
let retrieveX = module.getX;
retrieveX(); //9
module.getX(); // 81

```

`retrieveX()`를 호출할때 Scope가 전역으로 되어있기 때문에 `this.x`는 9이다.

객체로부터 메소드를 추출한 뒤 그 함수를 호출할때, 원본 객체가 그 함수의 `this`로 사용될 것이라 기대하는 것은 어려울 수 있기 때문에, 바인딩을 통해서 기대하는 값을 도출할 수 있다.

```js
let boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```

### bind()

`.bind(module)`의 동작 방식에 대해 살펴보자.

`bind(module)`가 호출되면 함수처럼 호출 가능한 '특수 객체(exotic object)'를 반환하고, 이 객체를 호출하면 `this`가 `module`로 고정된 `retrieveX()`가 반환된다.

따라서 `boundGetX`를 호출하면 `this`가 고정된 `retrieveX`를 호출하는 것과 동일한 효과를 본다

```js
let user = {
  firstName: "John",
  say(phrase) {
    alert(`${phrase}, ${this.firstName}!`);
  }
};

let say = user.say.bind(user);

say("Hello"); // Hello, John
say("Bye"); // Bye, John
```


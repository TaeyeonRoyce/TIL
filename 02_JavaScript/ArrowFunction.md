# Arrow Function

### 화살표 함수란?

화살표 함수는 전통적인 함수 표현의 간소화로, 간략한 방법으로 함수를 선언 할 수 있지만 제한사항이 생긴다.

- `this`나 `super`에 대한 바인딩이 없고, `methods`로 사용될 수 없습니다
- `new.target`키워드가 없습니다.
- 일반적으로 스코프를 지정할 때 사용하는 `call`, `apply`, `bind` methods를 이용할 수 없습니다.
- 생성자`(Constructor)`로 사용할 수 없습니다.
- 화살표 함수는 `prototype` 속성이 없습니다.



### 구문

```js
let pow1 = function (x) { return x * x; };
console.log(pow1(10)); // 100

let pow2 = x => x * x;
console.log(pow2(10)); // 100
```



#### callback에서의 사용

```js
var arr1 = [1, 2, 3];
var pow1 = arr1.map(function (x) { // x는 요소값
  return x * x;
});
console.log(pow1); // [ 1, 4, 9 ]

let arr2 = [1, 2, 3];
let pow2 = arr2.map(x => x * x);
console.log(pow2); // [ 1, 4, 9 ]
```



### this

일반함수와의 가장 큰 차이는 `this`이다. JS에서는 함수 호출 방식에 의해 `this`에 바인딩할 객체가 동적으로 결정된다. 선언이 아니라, 호출이 될때 결정된다는 것을 알아두자.

```js
function Prefixer(prefix)
{
  this.prefix = prefix;
}
Prefixer.prototype.prefixArray = function (arr)
{  // (A)
  return arr.map(function (x) {
    return this.prefix + ' ' + x; // (B)
  });
};
var pre = new Prefixer('Hi'); console.log(pre.prefixArray(['Lee', 'Kim']));
```

지금 살펴볼건,  화살표 함수의 `this` 언제나 상위 스코프의 `this`를 가리킨다라는 점을 유의하자. 화살표함수의 바인딩 객체 결정 방식이 렉시컬 스코핑과 비슷하다.

```js
function Prefixer(prefixA) {
  this.prefixB = prefixA;
}

Prefixer.prototype.prefixArray = function (arr) {
  // this는 상위 스코프인 prefixArray 메소드 내의 this를 가리킨다.
  return arr.map(x => `${this.prefixB}  ${x}`);
};

const pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));
//['Hi  Lee', 'Hi  Kim']
```



### 화살표함수를 피해야할 경우

메소드정의

```js
const person = {
  name: 'Lee',
  sayHi: () => console.log(`Hi ${this.name}`)
};

person.sayHi(); // Hi undefined
```

프로토타입

```js
const person = {
  name: 'Lee',
};

Object.prototype.sayHi = () => console.log(`Hi ${this.name}`);

person.sayHi(); // Hi undefined
```

addEventListener 함수의 콜백 함수

`() =>{}`  //  `this`가 `window`를 바인딩

`function() => {}` //  `this`가 `currentTarget`을 바인딩

일반함수로 사용하자

```js
var button = document.getElementById('myButton');

button.addEventListener('click', () => {
  console.log(this === window); // => true
  this.innerHTML = 'Clicked button';
});
```




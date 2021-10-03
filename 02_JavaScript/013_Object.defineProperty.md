# Object.defineProperty

`Object.defineProperty()` 는 객체에 새로운 속성을 직접 정의하거나 이미 존재하는 속성을 수정한 후, 해당 **객체**를 반환한다.

> Object.defineProperty(obj, prop, descriptor)

- `obj` 프로퍼티를 정의할 객체.
- `prop` 프로퍼티의 이름
- `descriptor` 새로 정의하거나 수정하려는 속성을 쓰는 객체

defineProperty() 메소드를 사용하면 프로퍼티의 속성을 상세하게 조절할 수 있고, `getter`나 `setter`를 사용하여 다른 객체나 변수에서 읽어올 수도 있다.

여기서`descriptor` 에는 `data descriptor` or `accessor descriptor` 로 나뉜다.

### data descriptor

`data descriptor` 에는 4가지 `key`가 있다.

- `configurable`: `props`의 값을 변경할 수 있고, 삭제할 수도 있다면 `true` // `false`가 default
- `enumerable`: `props`가 열거 된다면 `true` // `false`가 default
- `value`: `props`의 값 // `undefined`가 default
- `writable`: `props`의 값을 할당 연산자로 바꿀 수 있다면 `true` // `false`가 default

예제

```js
const object1 = {};

Object.defineProperty(object1, 'name', {
  value: 'Royce',
  enumerable: true,  // for (key in object1){ }  => 'Royce' 노출
  configurable: true,  // delete object1.name 가능
  writable: true  //object1.name = "TT" 가능
});

```



### accessor descriptor

`accessor descriptor` 에는 4가지 `key`가 있다.

- `get` 
- `set` 
- `configurable`
- `enumerable`

```js
const object1 = {};

let myName = "Royce";

Object.defineProperty(object1, "name", {
  enumerable: true,
  configurable: true,
  get() {
    return myName;
  },
  set(newName) {
    myName = newName;
  }
});

console.log(object1.name);  //Royce
object1.name = "TT";
console.log(object1); //TT
```



### props 수정하기

`Object.defineProperty()`가 수정하려고 하는 속성의 `configuarble` `false`면 수정이 불가능하다.

```js
let o = {};
Object.defineProperty(o, 'a', {
  get() { return 1; },
  configurable: false  //모든 특성을 변경 할 수 없다.
});

Object.defineProperty(o, 'a', {
  configurable: true
}); // TypeError

Object.defineProperty(o, 'a', {
  enumerable: true
}); // TypeError

Object.defineProperty(o, 'a', {
  set() {}
}); // TypeError (set이 undefined였음)

Object.defineProperty(o, 'a', {
  get() { return 1; }
}); // TypeError
// (형태는 같지만 서로 다른 함수이므로)

Object.defineProperty(o, 'a', {
  value: 12
}); // TypeError

console.log(o.a); // 1 기록
delete o.a; // 아무 일도 없음
console.log(o.a); // 1 기록
```
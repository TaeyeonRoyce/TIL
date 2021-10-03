# Map



### `map()` method

`map()` 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 **배열** 을 반환한다. 이때 기존의 함수는 변형되지 않는다.

`map()`은 반환값이 존재한다는 것을 잊지 말자.

### map 구문

`array.map(callback(currentValue, index, array), thisArg)`



- `callback() `: 새로운 배열 요소를 생성하는 함수 -> 결과의 배열을 반환
  - `currentValue `: 배열 내 현재 값
  - `index` `(optional)` : 배열 내 현재 값의 인덱스
  - `array` `(optional)`: 현재 배열
- `thisArg` `(optional)`: callback 내에서 this로 사용될 값

### 예제

```js
let numbers = [1, 4, 9];
let doubles = numbers.map(function(num) {
  return num * 2;
});
// doubles [2, 8, 18]
// numbers[1, 4, 9]
```

```js
let genres =['action', 'romance', 'thriller', 'animation'];

genres.map((genre, index) => (
  console.log(genre + " "+ index)
	)
)
// Arrow function도 활용 가능하다
// action 0
// romance 1
// thriller 2
// animation 3
```



# ForEach

`forEach()`메서드는 주어진 함수를 배열 요소 각각에 대해 실행한다. 반환값이 없다는 점이 `map()`과의 큰 차이점이다.

### forEach 구문

`array.forEach(callback(currentValue, index, array), thisArg)`

- `callback() `: 각 요소에 대해 실행할 함수 -> 반환 `undefined`
  - `currentValue `: 처리할 현재 요소
  - `index` `(optional)` : 처리할 현재 요소의 인덱스
  - `array` `(optional)`: `forEach()`를 호출한 배열
- `thisArg` `(optional)`: callback 내에서 this로 사용될 값

`forEach`는 중간에 멈추지 않고 호출한 배열의 요소에 대해 모두 `callback()`을 실행한다.



### 예제

```js
const items = ['item1', 'item2', 'item3'];
const copy = [];

for (let i=0; i<items.length; i++) {
  copy.push(items[i]);
}

items.forEach(function(item){
  copy.push(item);
});
```

```js
let genres =['action', 'romance', 'thriller', 'animation'];

genres.forEach(function(genre, index){
  console.log(genre);
    console.log(index);
 });
/*
action
0
romance
1
thriller
2
animation
3
*/
```


# List

### map() 활용

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
     <li>{number}</li>
);
                              
                              ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

이렇게 `JS`의 `map()`을 활용하여 여러게의`Component`를 배열로 반환받아 렌더링 할 수 있다.



좀 더 일반적인 코드

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>    			<li>{number}</li>
	);
	return (
    <ul>{listItems}</ul>
	);
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,  document.getElementById('root')
);
//key is required!
```

이 코드를 실행하면 리스트의 각 항목에 key를 넣어야 한다는 경고가 표시되는데, “key”는 엘리먼트 리스트를 만들 때 포함해야 하는 특성이다.

여기선 `key`에 `map()`이 제공하는 배열의 인덱스로 할당해 주어 해결 할 수 있다. `React Official`에서는 최후의 수단으로 사용하라 했다. `index`는 순서가 바뀌는 경우가 많지 때문에 권장하지 않는다고 한다.



### Key

`Key`가 필요한 이유는 `React`가어떤 항목을 변경, 추가 또는 삭제할지 식별하기 위해 필요하다. 대부분의 경우 데이터의 ID를 key로 사용한다.

경험상 `map()` 함수 내부에 있는 엘리먼트에 key를 넣어 주는 게 좋습니다. -React

`key`는 고유한 값이어야 하지만, 전체 범위에선 고유할 필요가 없다. 형제 사이의 범위에서만 고유하면 된다.



### JSX에서의 일반적인 활용

JSX를 사용하면 중괄호 안에 모든 표현식을 포함 시킬 수 있으므로 `map()` 함수의 결과를 인라인으로 처리한다.

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>        <ListItem key={number.toString()}
value={number} />
   	)}
    </ul>
  );
}
```
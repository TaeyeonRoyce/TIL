# useState

`Class Component`의 `state` 기능을 대체하는 것이 바로 `useState`이다.

`Class Component`와 `Function Component`를 비교하며 알아보자.

### State 변수 선언

```jsx
import React from 'react';

class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
```

```jsx
import React from 'react';
import { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);
}
```

`const [count, setCount] = useState(0);`

`count`라는 `state`변수 선언.

`count`의 값을 갱신하려면 `setCount`를 호출해야한다.

`useState(0)`은 `count`라는 `state`변수의 초기값을 0으로 할당한다.



### State 변수 사용

```jsx
//class component 
<p>You clicked {this.state.count} times</p>
```

```jsx
//function component 
<p>You clicked {count} times</p>
```



### State의 갱신

```jsx
//class component 
<button onClick={() => this.setState({ count:this.state.count + 1})}>
  Click me
  </button>
```

```jsx
//function component 
<button onClick={() => setCount(count + 1)}>    Click me
  </button>
```

`state`를 선언할때 같이 선언한`setCount`를 호출하여 `State`를 갱신할 수 있다.



### useState의 구조

`useState`를 이용하여 변수를 선언하면 2개의 아이템 쌍이 들어있는 배열로 만들어지는데, 첫 번째 아이템은 현재 변수를 의미하고, 두 번째 아이템은 해당 변수를 갱신해주는 함수이다. 그래서 `JS`의 구조 분배 할당을 통해 `state`와 `state`를 갱신하는 함수를 할당 할 수 있다.

```jsx
 const [fruit, setFruit] = useState('banana');
```


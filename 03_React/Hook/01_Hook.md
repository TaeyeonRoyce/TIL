# Hook

### Hook의 등장

React에는 Cmponent에는  `Class Componen`t와 `Function Component`두가지 타입이 있다. Hook의 등장이전에는 `state`나 `Life Cycle method`를 활용하려면 무조건 `Class Component`를 사용해야 한다. 그런데  `Class Component`의 단점들이 있다.

- 코드가 길고 복잡하다. `this`, `binding`, `constructor`
- 재사용이 어렵다.
- 퍼포먼스의 차이점

`State`나 `Life Cycle method`를 `Function Component`에서는 사용을 못하다 보니, `function component`로 작성하다 `state` 나 `Life Cycle method`를 사용하려면 `Class Component`로 변경해야했다.

이러한 문제들을 해결하기 위해 `Hook`이 등장했고 `React`에서 주어지는 `Hook` 뿐만아니라 `custom Hook`도 등장하며 널리 사용되고 있다.

코드 길이의 차이

`Class Component`

```jsx
import React from "react";

class App extends React.Component {
  state = {
      number: 0
  };
  render() {
    return (
      <div>
        <div>{this.state.number}</div>
        <button onClick={this.handleClickIncrement}>더하기</button>
        <button onClick={this.handleClickDecrement}>빼기</button>
      </div>
    );
  }
  handleClickIncrement = () => {
    this.setState(state => ({
      number: state.number + 1
    }));
  };
  handleClickDecrement = () => {
    this.setState(state => ({
      number: state.number - 1
    }));
  };
}

export default App;
```



`Function Component` (Hook)

```jsx
import React from "react";
import { useState } from "react";

const App = () => {
  const [number, setNumber] = useState(0);
  return (
    <div>
      <div>{number}</div>
      <button onClick={() => setNumber(number + 1)}>더하기</button>
      <button onClick={() => setNumber(number - 1)}>빼기</button>
    </div>
  );
};

export default App;
```

가독성 뿐만 아니라 길이 또한 줄어들었다.



### Hook의 규칙

1. 최상위 레벨에서만 `Hook`호출. 반복문, 조건문, 메소드 내에서 사용하지 말 것.
2. `React Function Compnent`내에서만 `Hook`호출하기. `Component`밖의 `JS`공간에서는 호출 하면 안된다고 한다.
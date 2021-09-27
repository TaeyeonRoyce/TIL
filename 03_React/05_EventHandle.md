# Event

React에서 event를 처리하는 방식이 js와 비슷하다.

- React의 이벤트는 소문자 대신 캐멀 케이스(camelCase)를 사용한다.
- JSX를 사용하여 문자열이 아닌 함수로 이벤트 핸들러를 전달한다.

이정도 차이를 React Official에서 말하는데,

```HTML
<!-- HTML with JS -->
<button onclick="activateLasers()">
  Activate Lasers
</button>
<!-- HTML with React.js -->
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

`onclick` -> `onClick`, `"activateLasers()"` -> `{activateLasers}`

### default값 처리하기

```HTML
<!-- HTML with JS -->
<form onsubmit="console.log('You clicked submit.'); return false">
  <button type="submit">Submit</button>
</form>
```

```jsx
<!-- HTML with React.js -->
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

`return false` => `preventDefault()`

여기서 사용된 `e` 는 합성 이벤트로 브라우저와 호환성이 보장됨.





### 실 사용 예제

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
    // 콜백에서 `this`가 작동하려면 아래와 같이 바인딩 해주어야 한다.
    this.handleClick = this.handleClick.bind(this);  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }
  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

여기 `constructor(props)`를 살펴보면,  `bind()`가 사용된걸 볼 수 있다. JS처럼 React 또한 클래스 메서드는 바인딩이 되어 있지 않기 때문에, 바인딩을 통해서 `this`의 정보가 소멸되기 전에 받아야 한다.



### EventHandler에 index전달하기 

> React Official

루프 내부에서는 이벤트 핸들러에 추가적인 매개변수를 전달하는 것이 일반적입니다. 예를 들어, `id`가 행의 ID일 경우 다음 코드가 모두 작동합니다.

```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
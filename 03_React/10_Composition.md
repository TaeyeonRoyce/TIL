# Composition

`React`에서는 합성을 사용하여 컴포넌트를 재사용하는 것을 권장.

상속보다는 합성

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}

function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

`{props.children}`을 통해 컴포넌트에 컴포넌트를 담을 수 있다.

`props`의 제한이 없다는 점.

또다른 예제

```jsx
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );  //props를 여러게 주어 원하는 부분에 component를 합성할 수 있다.
}
```


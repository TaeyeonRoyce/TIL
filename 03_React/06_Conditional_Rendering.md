# Conditional Rendering

React의 Component를 선택적으로 rendering 하기



### 기본 조건문



```jsx
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;  //false
}
ReactDOM.render(
  <Greeting isLoggedIn={false} />,  document.getElementById('root')
);
```



### inline으로 처리하기

```jsx
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>You have {unreadMessages.length} unread messages.</h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

> *JavaScript에서 `true && expression`은 항상 `expression`으로 평가되고 `false && expression`은 항상 `false`로 평가됩니다.*

### 삼항연산자로 처리

```jsx
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```



### Component rendering 막기

```jsx
function WarningBanner(props) {
  if (!props.warn) {
    return null;  //redering의 반환이 null인 경우에는 lifecycle에 영향을 주지 않음.
  }
  return (
    <div className="warning">
      Warning!
    </div>
  );
}
```
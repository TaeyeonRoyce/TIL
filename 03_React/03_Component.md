# Component

Component란 UI를 구성하는 재사용이 가능한 개별적인 조각으로 생각하면 편하다. 

Component에는

- 함수 Component
- 클래스 Component

두 가지가 존재하는데, `class component`가 일반적으로 기능이 많아 주로 사용되어 왔지만, React 16.8이후로 `function component`에서도 `state`와 `lifecycle`을 활용할 수 있게 되면서 적절히 병행하면서 사용한다. 큰 차이라면 `class component`는 `render()`메소드가 필수적이다

*Component는 항상 대문자로 시작해야한다*



## Component 사용

```jsx
function Welcome(props) {  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;ReactDOM.render(
  element,
  document.getElementById('root')
);
```

`const element`라는 엘리먼트가`<Welcom /> ` Component에 `name = "Sara" props를 가진 컴포넌트가 되었다` (`props`는 속성을 나타내는 데이터)

위 코드에서 `root` DOM에 `Hello Sara`가 나타날 것 이다.



앞서 말한 것 처럼 Component는 재사용이 가능하다.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
//출력
//Hello, Sara
//Hello, Cahal
//Hello, Edite
```



## Component lifecycle

Component가 UI를 구성할때 `Mounting(그리기)`,`Updating(갱신)`, `Unmounting(지우기)`이라는 과정을 거치는데, 각 프로세스마다 해당하는 lifecycle 함수가 실행됨.

## Props와 State

`Component`는 두 가지 인스턴스 속성이 있는데, `props`와 `state`가 있다.이렇게 둘로 나눈 이유는 `props`는 값이 할당 될 뿐, `component`내부에서 값이 변경될 수 없고, 변경이 필요한 값들은 `state`를 이용해야한다는 차이가 있다.

`props`가 존재하는 이유는 사용자 이벤트에 맞춰 변경되지 않아도 되는 값들을 분리하여 아래쪽 방향으로만의 영향을 생각하면 되고, `updating`을 할 경우 `state`에 존재하는 `key`들만 검사하면 되기 때문에 퍼포먼스 측면에서도 분리의 이유가 존재한다.



여긴 `props`를 활용한 `Component`

```jsx
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```



여긴 `state`를 활용한 `Component` *-class component를 사용*

```jsx
class Clock extends React.Component {
  //클래스 컴포넌트는 항상 props로 기본 constructor를 호출해야 함.
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is 
          {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,  document.getElementById('root')
);
```

여기선 `state`를 활용하기 위해 `class component`를 사용했지만, `hook`이나 `setState()`를 활용해서 function component에서도 사용이 가능하다.



## State 주의사항 -React Official

1. 직접 수정 X

   `setState()`를 사용하자

   ```jsx
   // Wrong
   this.state.comment = 'Hello';
   ```

   ```jsx
   // Correct
   this.setState({comment: 'Hello'});
   ```

2. `State` 업데이트는 비동기적일 수 있음.

   `setState()`호출을 단일 업데이트로 처리함.

   `this.props`와 `this.state`가 비동기적으로 업데이트될 수 있기 때문에 다음 state를 계산할 때 해당 값에 의존해서는 안된다.

   ```jsx
   // Wrong
   this.setState({
     counter: this.state.counter + this.props.increment
   });
   ```

   ```jsx
   // Correct
   this.setState(function(state, props) {
     return {
       counter: state.counter + props.increment
     };
   });
   ```

3. `State` 업데이트는 병합된다.

   ```jsx
     constructor(props) {
       super(props);
       this.state = {
         posts: [],
         comments: []
       };
     }
   componentDidMount() {
       fetchPosts().then(response => {
         this.setState({
           posts: response.posts
         });
       });
   
       fetchComments().then(response => {
         this.setState({
           comments: response.comments
         });
       });
     }
   ```

   병합은 얕게 이루어지기 때문에 `this.setState({comments})`는 `this.state.posts`에 영향을 주진 않지만 `this.state.comments`는 완전히 대체된다.

   

4. 데이터는 아래로 흐른다.

   모든 state는 항상 특정한 컴포넌트가 소유하고 있으며 그 state로부터 파생된 UI 또는 데이터는 오직 트리구조에서 자신의 “아래”에 있는 컴포넌트에만 영향을 미친다.
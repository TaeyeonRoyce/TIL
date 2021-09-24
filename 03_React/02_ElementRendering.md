# Element Rendering

## 1. Element

엘리멘트는 React의 가장 작은 단위.

React 엘리먼트는 호스트 객체를 그릴 수 있는 일반적인 자바스크립트 객체.

React 엘리먼트는 가볍고 호스트 객체에 직접적으로 관여하지 않고 단지 화면에 무엇을 그리고 싶은지에 대한 정보만 들어있다. -> DOM을 업데이트함

```jsx
const sampleElement = <h1>Hello, world</h1>;
```



## 2. DOM에 Rendering

index.html 에 id가 root인 `<div>`가 있다

> *React로 구현된 애플리케이션은 일반적으로 하나의 루트 DOM 노드가 있습니다*  -React 공식문서

```html
<div id="root"></div>
```

위에서 만들었던 "smapleElement"라는 Element를 id가 "root"인 DOM에 전달하려면 `ReactDom.render()`로 전달할 수 있다.

```Jsx
ReactDOM.render(sampleElement, document.getElementById('root'));
```



## 3. Rendering된 Element Update

React의 Element는 불변객체이므로 update를 하기 위해선 새로운 Element를 생성하고 render()하는 방법이 있다.

```jsx
//예시
function tick() {
  const smapleElement = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

위 예시에서 `tick()` 이라는 함수를 1초마다 작동하여 "smapleElement" 라는 Element의 `{new Date().toLocaleTimeString()}`를 생성하며 업데이트를 한다.

React DOM은 해당 엘리먼트와 그 자식 엘리먼트를 이전의 엘리먼트와 비교하고 DOM을 원하는 상태로 만드는데 필요한 경우에만 DOM을 업데이트한다. 변경된 부분만 업데이트 되는 장점이 있다.


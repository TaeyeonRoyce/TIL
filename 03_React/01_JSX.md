# JSX



React를 통해서 SPA를 표현할때 사용되는 문법

## 1. JSX란?

JavaScript를 확장한 문법이며 React에서 다루어 진다. HTML과 JavaScript를 같이 사용할 수 있다는게 특징. 결국 JS를 할 줄 알면 크게 어렵진 않을 것이다. 브라우저에서 실행되기 전에 바벨이 JS로 변환하여 사용.

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

이렇게 `<div>`라는 HTML 태그도 사용이 가능하다.

JSX가 사용되는 이유는, 컴포넌트가 렌더링 될때 일일이 함수를 사용하여 .createElement와 같은 작업들을 줄여 편하게 작업을 할 수 있게 해줄 뿐더러, 가독성 측면에서도 보기 쉽다.

> 위 코드와 동일한 JS표현

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```



## 2. JSX 규칙

일반적으로 JS문법을 따른다. 명명 규칙또한 JS의 camelCase를 따르며, 선언이나 함수, 루프, 조건식이 가능하다.

1. 속성은 항상 ""로 감쌀것.

2. 태그는 항상 닫혀있을 것.

   ```jsx
   const element = <img src="user.avatarUrl"></img>;
   ```

3. 형제 노드는 작성X

   `<div>` `<Fragment>` 로 감싸기

   ```jsx
   function App(){
     return
     <div>
       <h1>Hello, React!</h1>
       <h1>Hello, JavaScript!</h1>
     </div>
     )
   }
   export default App;
   ```

   

4. JS값은 render()에서 정의하고 return에서 사용

   ```jsx
   function App(){
     render(){
       const name = 'Josh Perez';
       return(
         <div>{name}</div>
       )
     }
   }
   ```



일반적인 표현

```jsx
const name = 'Josh Perez';
const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};
const element = <h1>Hello, {user.firstName} and {name} </h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);

//Hello, Harper and Josh Perez
```




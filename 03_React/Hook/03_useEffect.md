# useEffect



### Side Effect

`Side Effect`란 함수가 실행되면서 함수 외부에 존재하는 값이나 상태를 변경시키는 행위이다. 데이터 가져오기, 구독 설정하기, 함수 외부에 존재하는 전역변수 변경하기, 쿠키 저장하기, React컴포넌트의 DOM수정하기 등이 있다.

React에서는 `effect`를 두가지로 구분하였는데, `clean-up`이 필요한 `effect`와 필요하지 않은 `effect`로 나누었다. `clean-up`이란 `effect`를 실행한 뒤 추가로 코드를 실행하여 메모리 누수와 같은 처리해야하는 부분들을 처리하는 것이다. 보통 `componentWillUnmount()`에서 실행되는 부분이 `clean-up`에 해당된다

### useEffect

이러한 `side effect`들을 `function Component`에서 처리하기 위해 `useEffect`가 도입되었다. `Class component`의 `Life cycle`에 관련된 함수들도 처리할 수 있다.

`useEffect`는 기본적으로 첫번째 렌더링과 이후의 모든 업데이트에서 수행되기 때문에 필요한 경우에는 `clean-up`을 통해 `effect`를 탈출해야한다.

```jsx
useEffect(() => {
  return clean-up()
}),deps[])
//component가 Mount되거나,Update될때 useEffect ()=>{}가 실행.
//component가 Unmounte될때는 return() 실행
//deps[]를 지정하면 deps[]의 인덱스가 변경될때만 useEffect()를 실행
//deps == dependency
```



사용예제

```jsx
import React, { useState, useEffect } from 'react';

function exampleCom(){
  const [number, setNumber] = useState(0)
  
  useEffect(()=>{
    console.log("Hello always!");
    return () => {
      console.log("Bye Bye");
    }
  }); //component Mount, Update시 실행, Unmount시 return도 실행
  
  useEffect(()=>{
    console.log("Hello New Number" + number);
  }, [number]); //deps[number], number의 값이 변경될때만 실행
  
 
  return(
    <button onClick={() => setNumber(number + 1)}>Up! Up!</button>
  )
}
```








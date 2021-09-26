# Async / Await

Promise의 then chaining이 많아지면 가독성이 떨어지는 단점을 보완하기 위해 만들어짐. babel의 사용이 필요함



`async`라는 키워드를 함수 앞에 추가하여 Promise 객체를 반환하도록 변경 후 `await` 라는 키워드를 통해 Fullfilled된 상태의 결과만 반환하도록 한다.

```js
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```

`await`는 `async function`에서만 사용 가능

## 실용예제

```js
function fetchUser() {
  var url = 'https://jsonplaceholder.typicode.com/users/1'
  return fetch(url).then(function(response) {
    return response.json();
  });
}

function fetchTodo() {
  var url = 'https://jsonplaceholder.typicode.com/todos/1';
  return fetch(url).then(function(response) {
    return response.json();
  });
}

async function logTodoTitle() {
  var user = await fetchUser();
  if (user.id === 1) {
    var todo = await fetchTodo();
    console.log(todo.title); // delectus aut autem
  }
}
```

이렇게 Promise를 사용해서 `fetchUser()` 와 `fetchTodo()`를 통해 정보를 받아온뒤,  `logTodoTitle()`을 실행하는 순서로 활용 가능.


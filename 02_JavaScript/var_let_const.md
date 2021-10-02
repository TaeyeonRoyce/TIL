# Declare variables

JS에는 `var`, `let`, `const`라는 키워드를 통해서 변수 선언을 할 수 있었다. `ES6` 부터 `let`과 `const`가 추가되었다.

### var

`var`는 정말 이상한 녀석이다.

```js
var a = "new";
var a = "old";
```

이렇게 변수를 중복해서 할당할 수 있고,

```js
while 1:
	var a = "new";
  if (a === "new"){
    break;
  }
console.log(a); //new

```

`var`로 선언한 변수는 함수에서만 지역변수일뿐 모두 전역변수로 선언된다. 

그리고 가장 이상한건 `hoisting` 과정에서 발견된다.

JS가 `var`를 `hoisting`할때, `var a`를 먼저 선언해두고, `undefined`로 초기화한다. 

```js
console.log(a); //undefined
var a = "new";
console.log(a); //a

```

그래서 위 코드에서는 `hoisting`이 이루어지고 나서 `var a = "new"`를 만나기 전까지 `undefined`로 존재한다.

# let, const

`let`과 `const`는 굉장히 일반적인 변수(let), 상수(const) 이다. `var`는 사용될 이유가 없다.

코드가 직관적으로도 이해하기 힘들어지고, 여러 이슈를 야기 할 수 있다. 또, 재할당이 필요없는 변수에 대해서도 무조건 재할당 가능 변수로 선언해야 하기 때문에 좋지않다.

재할당이 필요하면 `let`, 아니면 `const`를 사용하자.

MDN에서 추천해준 `Airbnb JS guideline`에서 참조`References`에 관한 문서를 보면, 

- Use `const` for all of your references; avoid using `var`.
- If you must reassign references, use `let` instead of `var`

라고 되어있다. `const` > `let`순으로 사용하려고 하고, `var`는 쓰지 말라고 되어있다.
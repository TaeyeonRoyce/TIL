# Closure



## Clousure란?

클로저는 함수와 함수가 선언된 어휘적 환경의 조합.

>“A closure is the combination of a function and the lexical environment within which that function was declared.”
>
>클로저는 함수와 그 함수가 선언됐을 때의 렉시컬 환경(Lexical environment)과의 조합이다.   -MDN
>
>**Lexical Scope는 함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 결정된다*

클로저는 객체가 어떤 데이터와 그 데이터를 조작하는 함수의 연관에 기여하기 때문에 객체지향 프로그래밍과 같은 맥락에 있다.



```js
function outerFunc() {
  var x = 10;
  var innerFunc = function () { console.log(x); };
  return innerFunc;
}

/**
 *  함수 outerFunc를 호출하면 내부 함수 innerFunc가 반환된다.
 *  그리고 함수 outerFunc의 실행 컨텍스트는 소멸한다.
 */
let inner = outerFunc();
inner(); // 10
```

함수 `outerFunc`는 내부함수 `innerFunc`를 반환하고 생을 마감했다. 그런데 `inner`에는 10이 저장되어 있다. 

외부 함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접근할 수 있는데 이러한 함수를 클로저(Closure)라고 부른다. 클로저는 자신이 생성될 때의 환경(Lexical environment)을 기억하는데, 이때 외부함수의 변수를 참조하며 그 변수와 엮여있는 함수가 된다.



## Closure의 활용

#### 1. 현재 상태를 기억하고 변경된 최신 상태를 유지하기 위해

```js
var box = document.querySelector('.box');
var toggleBtn = document.querySelector('.toggle');

var toggle = (function () {
  var isShow = false;

  // 1. 클로저를 반환
  return function () {
    box.style.display = isShow ? 'block' : 'none';
    // 3. 상태 변경
    isShow = !isShow;
  };
})();

// 2. 이벤트 프로퍼티에 클로저를 할당
toggleBtn.onclick = toggle;
```



`HTML`에 `<button class="toggle"></button>` `<div class="box"></div>`  가 있다고 하자.

이 `button`이 `onclick()`되면 `toggle = (function(){})`이 실행된다.

여기서 순서가 중요한데, `toggle`함수가 먼저 `closure`함수를 반환하여 `toggleBtn`에 할당한다. 그리고 `toggleBtn`에서 `closure`함수가 기억하는 변수`isShow`가 소멸되지 않고 `toggle`함수의 렉시컬 환경을 기억한 상태로 유지한다. 변경되는 상태를 유지하는데 유용하게 작용한다. 만일 `closure`함수를 사용하지 않는다면 전역변수를 사용하여 많은 부작용이나 오류를 유발할 수 있다.

#### 2. 전역 변수의 사용 억제

MDN에선 `private method` 흉내내기 라고 한. 다른 언어에서는 `private`이라는 키워드를 선언하여 같은 클래스내에서만 해당 메소드가 호출 될 수 있게 한다.

```js
    var counter = (function() {
      var privateCounter = 0;
      function changeBy(val) {
        privateCounter += val;
      }
      return {
        increment: function() {
          changeBy(1);
        },
        decrement: function() {
          changeBy(-1);
        },
        value: function() {
          return privateCounter;
        }
      };
    })();

    console.log(counter.value()); // logs 0
    counter.increment();
    counter.increment();
    console.log(counter.value()); // logs 2
    counter.decrement();
    console.log(counter.value()); // logs 1
```

여기서는 `counter.increment`, `counter.decrement`, `counter.value `세 함수에 의해 공유되는 환경이 생성된다. 이 환경은 `closure` 함수에서 만들어 지는데,  `privateCounter`라는 변수와 `changeBy`라는 함수가 `closure` 함수 외부에서 접근될 수 없는 `private`한 메소드가 되었다.

#### 3. 캡슐화

```js
var makeCounter = function() {
      var privateCounter = 0;
      function changeBy(val) {
        privateCounter += val;
      }
      return {
        increment: function() {
          changeBy(1);
        },
        decrement: function() {
          changeBy(-1);
        },
        value: function() {
          return privateCounter;
        }
      }
    };

    var counter1 = makeCounter();
    var counter2 = makeCounter();
    alert(counter1.value()); /* 0 */
    counter1.increment();
    counter1.increment();
    alert(counter1.value()); /* 2 */
    counter1.decrement();
    alert(counter1.value()); /* 1 */
    alert(counter2.value()); /* 0 */
```

두 개의 카운터가 어떻게 다른 카운터와 독립성을 유지하는지 주목해보자. 각 클로저는 그들 고유의 클로저를 통한 `privateCounter` 변수의 다른 버전을 참조한다. 각 카운터가 호출될 때마다; 하나의 클로저에서 변수 값을 변경해도 다른 클로저의 값에는 영향을 주지 않는다.



#### 주의

```HTML
    <p id="help">Helpful notes will appear here</p>
    <p>E-mail: <input type="text" id="email" name="email"></p>
    <p>Name: <input type="text" id="name" name="name"></p>
    <p>Age: <input type="text" id="age" name="age"></p>
```

```js
   function showHelp(help) {
      document.getElementById('help').innerHTML = help;
    }

    function setupHelp() {
      var helpText = [
          {'id': 'email', 'help': 'Your e-mail address'},
          {'id': 'name', 'help': 'Your full name'},
          {'id': 'age', 'help': 'Your age (you must be over 16)'}
        ];

      for (var i = 0; i < helpText.length; i++) {
        var item = helpText[i];
        document.getElementById(item.id).onfocus = function() {
          showHelp(item.help);
        }
      }
    }

    setupHelp();
```

`for`에서 세 개의 클로저가 만들어졌지만 각 클로저는 (`item.help`) 라는 같은 단일 환경을 공유하기 때문에 각각의 `id`에 적용되지 않고, `for`의 마지막 `item.id`인 `Age`에 해당할때의 경우만 동작한다.

이런 경우는 더 많은 클로저를 분리하여 `Scope`이 겹치지 않게 분리해 주자.

```js
 function showHelp(help) {
      document.getElementById('help').innerHTML = help;
    }

    function makeHelpCallback(help) {
      return function() {
        showHelp(help);
      };
    }
 //각각의 콜백에 새로운 환경을 생성하여 분리 작동이 가능
    function setupHelp() {
      var helpText = [
          {'id': 'email', 'help': 'Your e-mail address'},
          {'id': 'name', 'help': 'Your full name'},
          {'id': 'age', 'help': 'Your age (you must be over 16)'}
        ];

      for (var i = 0; i < helpText.length; i++) {
        var item = helpText[i];
        document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
      }
    }

    setupHelp();
```



### ` let` solution

```js
    function showHelp(help) {
      document.getElementById('help').innerHTML = help;
    }

    function setupHelp() {
      var helpText = [
          {'id': 'email', 'help': 'Your e-mail address'},
          {'id': 'name', 'help': 'Your full name'},
          {'id': 'age', 'help': 'Your age (you must be over 16)'}
        ];

      for (var i = 0; i < helpText.length; i++) {
        let item = helpText[i];
        document.getElementById(item.id).onfocus = function() {
          showHelp(item.help);
        }
      }
    }

    setupHelp();
```

위의 경우 `var` 대신 `let`을 사용하여 모든 클로저가 블록 범위 변수를 바인딩할 것이므로 추가적인 클로저를 사용하지 않아도 완벽하게 동작할 것이다.  -> binding과 let 공부하기



### 성능 관련 고려사항

특정 작업에 클로저가 필요하지 않는데 다른 함수 내에서 함수를 불필요하게 작성하는 것은 현명하지 않다. 이것은 처리 속도와 메모리 소비 측면에서 스크립트 성능에 부정적인 영향을 미칠 것이다.

예를 들어, 새로운 객체/클래스를 생성 할 때, 메소드는 일반적으로 객체 생성자에 정의되기보다는 객체의 프로토타입에 연결되어야 한다. 그 이유는 생성자가 호출 될 때 마다 메서드가 다시 할당되기 때문이다 (즉, 모든 개체가 생성 될 때마다).
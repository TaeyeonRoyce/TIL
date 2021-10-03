# Module



## Module이란?

**여러 기능들에 관한 코드가 모여있는 하나의 파일**

하나의 파일에 코드를 전부 작성하는 것보단, 각각의 기능들과 UI에 따라 각각의 모듈로 작성하는 것이 더욱 효율적이다. 의존성이 줄어들어 수정이나 개선에 용이하고, 재사용이 가능하다는 점에서 효율적이라고 할 수 있다. 또한 가독성도 매우 높아진다.

모듈을 활용하면 다른 파일에 있는 코드를 동일 파일내에 정의 한 것처럼 기능을 사용할 수 있다.

모듈을 사용하기 위해선 내보내기와 불러오는 과정이 필요하다.

## Export & Import (ES6)

> *`export`와 `import` 모듈은 항상 `use strict` 모드이다.*



`export`의 방식에는 `named`와 `defalut`가 있다.

### `named export`

```js
// 하나씩 내보내기
export let name1, name2, …, nameN; //const도 동일
export let name1 = …, name2 = …, …, nameN; //const도 동일
export function functionName(){...}
export class ClassName {...}

// 목록으로 내보내기
export { name1, name2, …, nameN };

// 내보내면서 이름 바꾸기
export { variable1 as name1, variable2 as name2, …, nameN };

// 비구조화로 내보내기
export const { name1, name2: bar } = o;
```

코드에서 살펴보듯이 `let, const, class, function` 앞에서 `export` 키워드를 활용하여 내보낼 수 있다.

`export { name1, name2, …, nameN };`처럼 한번에 여러가지 요소를 내보낼 수 있다.

주의할 점이 있다면, `named export`을 받을땐, 반드시 내보낸 이름과 동일한 이름을 사용해야한다.

```js
// module "my-module.js"
function cube(x) {
  return x * x * x;
}
const foo = Math.PI + Math.SQRT2;
let graph = {
    options: {
        color:'white',
        thickness:'2px'
    },
    draw: function() {
        console.log('From graph draw function');
    }
}
export { cube, foo, graph };
```

```js
// "main.js"
import { cube, foo, graph } from 'my-module';
graph.options = {
    color:'blue',
    thickness:'3px'
};
graph.draw();
console.log(cube(3)); // 27
console.log(foo);    // 4.555806215962888
```

### `default export`

`named export`와 다른점이라면 단일 값을 내보내고, 불러올 때 꼭 같은 이름으로 사용해야할 필요가 없다는 점이다. `default export`는 단일 값을 보내지만, 전체를 감싸 내보내고 불러오면 여러 값들을 사용할 수 있다.

```js

export default{
  function cube(x) {
    return x * x * x;
  },
  const foo = Math.PI + Math.SQRT2,
  let graph = {
      options: {
          color:'white',
          thickness:'2px'
      },
      draw: function() {
          console.log('From graph draw function');
      }
  }
}
```

```js 
// "main.js"
import ojcInMain from 'my-module';
ojcInMain.graph.options = {
    color:'blue',
    thickness:'3px'
};
ojcInMain.graph.draw();
console.log(ojcInMain.cube(3)); // 27
console.log(ojcInMain.foo);    // 4.555806215962888
```



### `import`

```js
//모듈내의 export한 모든 것들을 myModule로 불러오기(바인딩되어 들어간다)
import * as myModule from "my-module.js";

//모듈 내의 myMember를 불러오기
import {myMember} from "my-module.js";

//모듈 내의 reallyReallyLongModuleMemberName shortName으로 불러오기
import {reallyReallyLongModuleMemberName as shortName} from "my-module.js";

//default export된 내용을 myModule로 불러오기
import myModule from "my-module.js";

//default import와 named import 병행
import myDefault, {foo, bar} from "my-module.js";

```



예시

```js
// --file.js--
function getJSON(url, callback) {
  let xhr = new XMLHttpRequest();
  xhr.onload = function () {
    callback(this.responseText)
  };
  xhr.open("GET", url, true);
  xhr.send();
}

export function getUsefulContents(url, callback) {
  getJSON(url, data => callback(JSON.parse(data)));
}

// --main.js--
import { getUsefulContents } from "file.js";
getUsefulContents("http://www.example.com", data => {
  doSomethingUseful(data);
});
```





## HTML에서의 모듈

> - index.html
> - main.js
> - modules/
>       canvas.js
>       square.js

위 구조에서 HTML에 모듈을 적용한 main.js를 선언하려면

```html
<script type="module" src="main.js"></script>
```

`type= "module"`을 선언해야한다.



## `Module`의 특성

### module aggregation

모듈도 종속성을 가진다. 

> - modules/
>     canvas.js
>     shapes.js
>     shapes/
>       circle.js
>       square.js
>       triangle.js

위 구조에서 `circle.js`,`square.js`,`triangle.js`를 `shape.js`로 내보내서 모으면, `main.js`에서 `shape.js`만 불러와도 `circle.js`,`square.js`,`triangle.js`를 사용할 수 있다.

코드로 살펴보면, 

```js
//circle.js, square.js, triangle.js
export { Square };
export { Circle };
export { Triangle };
```

```js
//shape.js
export { Square } from './shapes/square.js';
export { Triangle } from './shapes/triangle.js';
export { Circle } from './shapes/circle.js';
```

```js
//main.js
import { Square } from './modules/square.js';
import { Circle } from './modules/circle.js';
import { Triangle } from './modules/triangle.js';
```



### Dynamic module loading (동적 모듈 로딩)

가장 큰 이점은, 필요할 때만 부분적으로 모듈을 불러올 수 있다는 점이다.

```js
let squareBtn = document.querySelector('.square');

squareBtn.addEventListener('click', () => {
  import('/js-examples/modules/dynamic-module-imports/modules/square.js').then((Module) => {
    let square1 = new Module.Square(myCanvas.ctx, myCanvas.listId, 50, 50, 100, 'blue');
    square1.draw();
    square1.reportArea();
    square1.reportPerimeter();
  })
});
```

`module object`가 `promise`를 반환하게 하여 동적으로 로드되도록 구현

*`promise fulfillment`가 모듈 객체를 반환하기 때문에 클래스는 객체의 하위 기능으로 만들어집니다. 따라서 이제 `Module` 을 사용하여 생성자(contructor)에 접근해야 합니다. 예를들어 `Module.Square( ... )` 와 같이 앞에 `Module`이 붙습니다. -MDN*


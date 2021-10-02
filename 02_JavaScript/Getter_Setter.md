# Getter Setter

객체의 프로퍼티에는 두 종류가 있는데, `data property` 와 `accessor property`가 있다. 

````js
let obj ={
  name: "Royce"
};
````

평소에 사용하는 프로퍼티가 `data property`이다.

`accessor property`는 값을 `get`하고 `set`하는 역할이 있다. `accessror property`의 본질은 함수이다.



## accessor property

```js
let obj = {
  get propName() {
    //obj.propName을 실행할 때 실행되는 코드
  },

  set propName(value) {
    //obj.propName = value를 실행할 때 실행되는 코드
  }
};
```

#### 1. get

어떤 프로퍼티에 접근할 때 실행되는 메소드나 코드는 `getter`를 이용하자.

`getter`메소드는 매개변수를 할당 할 수 없고, 읽기만 가능하다. `getter`나 `setter`메소드를 사용할땐 비록 본질은 함수이나 객체의 `property`를 사용하는 것처럼 사용하면 된다.

```js
let obj = {
  log: ['example','test'],
  get latest() {
    if (this.log.length == 0) return undefined;
    return this.log[this.log.length - 1];
  }
}
console.log(obj.latest); //"test".
//obj.latest = "test"; ERROR
```

`obj.latest`를 통해서 `get`메소드를 이용했다.

`getter`는 프로퍼티에 접근하기 전까지는 값을 계산하지 않는다. 그래서 `getter`를 사용하여 메모리를 save할 수 있다.

MDN에선 아래와 같은 상황에서 유용하다고 한다.

- 값의 계산 비용이 큰 경우. (RAM이나 CPU 시간을 많이 소모하거나, worker thread를 생성하거나, 원격 파일을 불러오는 등)
- 값이 당장은 필요하지 않지만 나중에 사용되어야 할 경우, 혹은 절대로 이용되지 않을 수도 있는 경우.
- 값이 여러 차례 이용되지만 절대 변경되지 않아 매번 다시 계산할 필요가 없는 경우, 혹은 다시 계산되어서는 안 되는 경우.

### 2. set

`setter`는  특정한 속성에 값이 변경되어 질 때마다 함수를 실행하는데 사용된다.  *한 개의 매개변수만 가질 수 있다*

```js

let user = {
  name: "nothing",

  get changeName() {
    this.name = "getter";
  },

  set changeName(value) {
    this.name = value;
  }
};

console.log(user.name);// nothing
user.changeName; // get changeName 실행
console.log(user.name); // getter
user.changeName = "setter"; // set changeName 실행
console.log(user.name); // setter
```

이렇게 getter와 setter 메서드를 구현하면 객체엔 `changeName`이라는 '가상’의 프로퍼티가 생깁니다. 가상의 프로퍼티는 읽고 쓸 순 있지만 실제로는 존재하지 않는다.



## 프로퍼티 통제

`getter`와 `setter`를 ‘실제’ 프로퍼티 값을 감싸는 래퍼(wrapper)처럼 사용하면, 프로퍼티 값을 원하는 대로 통제하기 용이하다

아래 예시에선 `name`을 위한 setter를 만들어 `user`의 이름이 너무 짧아지는 걸 방지하고,'실제' 값은 별도의 프로퍼티 `_name`에 저장한다.

```js
let user = {
  get name() {
    return this._name;
  },

  set name(value) {
    if (value.length < 4) {
      alert("입력하신 값이 너무 짧습니다. 네 글자 이상으로 구성된 이름을 입력하세요.");
      return;
    }
    this._name = value;
  }
};

user.name = "Pete";
alert(user.name); // Pete

user.name = ""; // 너무 짧은 이름을 할당하려 함
```

*`user._name`을 사용해 이름에 바로 접근할 수 있지만 밑줄 `"_"` 로 시작하는 프로퍼티는 객체 내부에서만 활용하고, 외부에서는 건드리지 않는 것이 관습이다.*


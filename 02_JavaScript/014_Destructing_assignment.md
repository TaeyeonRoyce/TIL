# Destructing Assign

### 구조 분해 할당

구조 분해 할당이란 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 표현식.

함수에 객체나 배열을 전달해야 하는 경우, 저장된 데이터의 일부만 분해하여 전달할 수 있음.

### 배열에서의 구조 분해 할당

```js
let arr = ["TaeYeon", "Royce", "Won"]

// 구조 분해 할당을 이용
let [KrName, EnName] = arr;

console.log(KrName); // TaeYeon
console.log(EnName);  // Royce

//대안 
// let [KrName, EnName] = "TaeYeon Royce".split(' ');


//구조 분해 할당 과정에서는 기존 데이터를 보존.
console.log(arr); //["TaeYeon", "Royce", "Won"]

//, , 쉼표를 사용하여 필요하지 않은 배열 요소 무시
let [firstName, ,lastName] = arr;
console.log(firstName); //TaeYeon
console.log(lastName); //Won


```



#### 변수교환

```js
let guest = "TaeYeon";
let host = "Royce";
[guest, host] = [host, guest];

console.log(guest); //Royce
console.log(host); //TaeYeon

```



#### 슬라이싱

```js
let [fruit1, fruit2, ...rest] = [
  "Apple",
  "Banana",
  "Coconut",
  "Orange",
  "Grape"
];
console.log(fruit1); // Apple
console.log(fruit2); // Banana

// `rest`는 배열입니다.
console.log(rest); //["Coconut", "Orange", "Grape"]

```



### 객체에서의 구조 분해 할당

```js
let options = {
  name: "Royce",
  sex: "Male",
  age: 23
};

//배열과는 다르게 순서와 상관없이 상응하는 변수에 할당됨
let {sex, name, age} = options;

console.log(name);  // Royce
console.log(sex);  // Male
console.log(age); // 23

//다른 이름의 변수로 저장
// { 객체 프로퍼티: 목표 변수 }
let {name: Na, sex: Se, age: Ag} = options;


console.log(Na);  // Royce
console.log(Se);  // Male
console.log(Ag); // 23
```





### 함수에서의 사용

```js
// 함수에 전달할 객체
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

// options를 원하는 대로 분해 할당
// {title} = options, {items} = options
function showMenu({title, width = 200, height = 100, items}) {
  alert( `${title} ${width} ${height}` ); // My Menu 200 100
  alert( items ); // Item1, Item2
}

showMenu(options); //분해될 객체를 전달
```


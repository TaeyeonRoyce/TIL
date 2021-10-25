# Pseudo Element

### 가상 요소

별도의 class를 지정하지 않아도 요소를 선택한다는 개념

실제 HTML 페이지에 요소가 존재 하지는 않지만, 요소가 추가된 것과 같은 효과를 내는 CSS 선택자

예)

```html
<style>
  .HelloWorld .Hello::first-letter{
  	font-size: 80px;
  }
</style>

<div class="HelloWorld">
  <div class="Hello">Hello!</div>
  <div class="World">World!</div>
</div>

```

위 결과는 Hello 클래스의 H만의 크기만 80px로 나타난다.

가상요소를 활용하여  `first-letter`와 같은 키워드를 통해 존재하지 않은 요소를 선택할 수 있음

### 문법

`선택자::키워드 {}`,  *`.HelloWorld .Hello::first-letter{}`*

### 키워드

`::before` :  선택자 앞에 생성되는 자식요소

`::after` : 선택자 뒤에 생성되는 자식요소

`::first-line` : 텍스트의 첫 라인만 선택

`::first-letter` : 텍스트의 첫 글자만 선택

`::selection`: 사용자가 드래그로 선택한 블록



### ::before, ::after

**`content`**라는 속성이 꼭 필요하다.

HTML 문서 안에 반복적으로 나오는 같은 태그 앞에, 또는 반복되는 목록, 문단 앞이나 뒤에 공통으로 적용할 텍스트 내용, 아이콘, 배경 이미지 등을 표시할 때 유용하다.

```html
<style>
  .Hello::before{
    content:"→ ";
    color: #a00;
  }
  .World::before{
    content:" ←";
    color: #a00;
  }
</style>

<div class="Hello"> Hello </div>
<div class="World"> World </div>


<!--  → Hello   -->
<!--  Hello ←  -->
```



### ::first-line, ::first-letter

**블록 타입**의 요소에만 사용할 수 있다.

```html
<style>
  .Hello::first-letter {
    color: #ff0000;
    font-size: 200%;
  },
  /* T만 200%, color: #ff0000 */
  .World::first-letter {
    color: #ff0000;
    font-size: 200%;
  }
  /* "World" 모두 200%, color: #ff0000 */
</style>
<div class="Hello"> Hello </div>
<div class="World"> World </div>
```



### ::selection

원하는 부분이 드래그 되었을때 스타일 설정

```html
<style>
.Hello::selection{
    background-color: #0a0;
    color: #fff;
}
</style>
<div class="Hello"> Hello </div>
```


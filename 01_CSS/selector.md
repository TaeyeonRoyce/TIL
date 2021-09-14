## CSS Selector

##### HTML에 존재하는 요소들을 CSS에서 다루기 위해선 그 요소들을 선택하여야한다.

##### 선택자 우선순위를 고려해서 적용 범위를 조작하여 좀 더 효율적으로 스타일을 적용시키자

### 1. 선택방법

- tag  -> tag
- id -> #id
- class -> .class

```html
<html>
<head></head>
<body>
  <div id="buttonBox">
    <button class="up">clickMe</button>
    <button class="down">DoubleClickMe</button>
  </div>
</body>
</html>
<style>
  <!-- buttonBox 내부의 모든 button태그에 대해 빨간색배경 -->
  #buttonBox button{
   background-color : red; 
  }  
  
  <!-- buttonBox 내부의 up클래스에 대해 파란색배경 -->
  #buttonBox .up{
   background-color : blue; 
  }
</style>
```

선택자를 활용해서 원하는 요소를 선택할 수 있다.

자식요소는 단순히 공백을 활용하여 선택할 수 있다.

여기서 의문이 생긴다.

<button class="up"></button>이라는 요소가 두번 선택되었는데, 하나는 빨간색 다른하나는 파란색의 배경값을 주었다. 

<button class="up"></button>는 무슨색일까?

### 2. 선택자 우선순위

#### CSS에선 Specificity(명시도)기준으로 적용된다.

그 순서로는(명시도가 높은 순서)

1. !important 
2. 인라인 선택자     (style = "")
3. id 선택자           (#idName)
4. class 선택자      (.className)
5. 가상 클래스        (.up:hover)
6. 태그 선택자        (button)
7. 전체 선택자        (*)

##### 쉽게 생각해보면, 그 규정의 범위가 구체적이고 좁은 경우 일 수록 명시도가 높다

같은 우선 순위에 있는 경우, 부모-자식 관계가 많은 경우가 우선되며, 모든 설정이 같은 경우 **나중**에 선언한 것이 우선되어 적용됩니다.

MDN에서 말하길, !important의 사용은 **나쁜습관**이라고 한다. 중간에 톡! 하고 1순위로 자리잡아 자연스런 종속을 깨뜨려 버리기 때문이다.

아까의 예시에서 보면, 태그선택자를 활용하여 빨간색배경을 주었기 때문에 그보다 명시도가 높은 class의 스타일이 적용될 것이다.

그렇다고 태그선택자를 활용한 스타일이 무시되는것은 아니고, 중복되는(혹은 충돌) 스타일에 대해서만 덮어씌어지는 것이니 잘 활용해보자.

### 3. 예외

우리가 선택자를 통해 요소들을 집합시켰다면, 그 중 예외를 선택하는 것도 좋을 것 같다.

 ```css
 #buttonBox button:not(.down){
 background-color : red; 
 }  ```
 ```

:not() 을 활용하여 선택에서 배제시킬 순 있다.

not으로 예외를 하는 것과 예외할 요소에 대해 따로 정의해주는것의 차이가 없는것 같다.

차라리 명확한 class나 id 선언으로 예외사항도 같이 묶어버리는게 좋지 않을까 하는 생각이다.












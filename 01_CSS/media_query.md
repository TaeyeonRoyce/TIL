# Media Query

#### 미디어 쿼리는 화면에 반응적으로 다른 스타일을 적용하는데 유리하다. 화면크기나 단말 기기에 따른 다른 스타일을 적용할 수 있다.

###### 반응적으로 다르게 적용하려면, 어느 조건을 캐치해서 그에 맞은 처리를 해야할 것이다.



## 미디어 쿼리 기본

### 1. 미디어 유형

`all`  모든 장치

`print`  인쇄 결과물, 미리보기 중인 경우 

`screen` **화면크기에 따른 분류**

`speech` 음성 합성장치(음성 출력 전용 장치)

### 2. 미디어 특성

`width` 뷰포트의 width

`height` 뷰포트의 height

`orientation` 뷰포트의 방향 (모바일에서 가로, 세로)

`...` 다양한 특성이들이 있다. MDN에서 확인하자.

### 3. 논리 연산자

`and`,  `not`  ,`only`, `,(or) `

ex)

```CSS
@media (min-width: 30em) and (orientation: landscape) {...}
/* -> screen의 크기가 최소30em, 가로방향일 경우 {...} */
```

```css
@media screen and (min-width: 30em) and (orientation: landscape) {...}
/* -> view가 있는 미디어 기기에 screen의 크기가 최소30em, 가로방향일 경우 {...} */
```



```CSS
@media (min-height: 680px), screen and (orientation: portrait) { ... }
/* -> 최소높이가 680px 이거나 화면이 세로로 긴 모드일 때 스타일일 경우 {...}*/
```



```CSS
@media screen and (min-width:480px) and (max-width:768px) {...}
/* -> 뷰포트의 넓이가 480px - 768px일 경우 {...}
```



### > 사용

보통 PC화면에선 이렇게 구분한다.

```Css
@media screen and (max-width:1200px) { ... }
@media screen and (max-width:768px) { ... }
@media screen and (max-width:480px) { ... }
```



미디어 쿼리를 에러없이 사용하려면 viewport를 지정해줘야한다. 모바일 화면에서 볼 경우, 픽셀과 dpi에 따라 불러오는 이미지가 축소되기 때문에 가독성이 떨어지거나, 모바일 화면에 적합한 확대 축소나 웹에서 자동적으로 이루어 지는 것들이 모바일에서 불편할 경우가 있기 때문이다.

https://maen2001.tistory.com/22  - 빅범님

해당 사이트에 모바일 웹 제작시 meta태그에 대한 자세한 설정 및 설명이 되어있다.

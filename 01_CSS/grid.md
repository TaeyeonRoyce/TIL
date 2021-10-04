# Grid

CSS 레이아웃에는 `flex`와 `grid`가 있다.

`flex`가 한 방향의 레이아웃 시스템이라면 `grid`는 가로와 세로방향으로 레이아웃을 갖는 시스템이다.

`flex`보다 더 복합적인 레이아웃을 잡을 수 있다.

기초적인 구성은 `flex`와 비슷하다.



자주 사용되는 속성

1. Grid Container

### display

```css
.container {
	display: grid;
}
```

### set columns row

```css
.container {
	grid-template-columns: 200px 200px 500px;
	/* 
  grid-template-columns: 1fr 1fr 1fr // 1: 1: 1
	grid-template-columns: repeat(3, 1fr)  // 1fr 1fr 1fr
	grid-template-columns: 100px 200px auto
	*/
}

.container {
	grid-template-columns: repeat(3, 1fr);
	grid-template-rows: repeat(3, minmax(100px, auto));
  /* 최소 100px */
}
```

### gap

```css
.container{
  row-gap: 10px;
	/* row의 간격을 10px로 */
	column-gap: 20px;
	/* column의 간격을 20px로 */
  gap: 10px 20px;
	/* row-gap: 10px; column-gap: 20px; */
}
```

### auto set

```css
.container {
	grid-auto-rows: minmax(100px, auto); /* 개수와 상관없이 셀 생성 */
}
```



### align-items, justify-items

```css
.container {
	align-items: start; /* column axias */
	justify-items: stretch; /* row axias */
  /*
  align-content, justify-content: start;
  align-content, justify-content: center;
  align-content, justify-content: end;
  */
  place-items: start stretch;
} 
```



### align-content, justify-content

```css
.container {
  align-content: start; /* column axias */
  justify-content: stretch; /* row axias */
	/*
  align-content, justify-content: strech;
  align-content, justify-content: start;
  align-content, justify-content: center;
  align-content, justify-content: end;
  align-content, justify-content: space-between;
  align-content, justify-content: space-around;
  align-content, justify-content: space-evenly;
  */
	place-content: space-between center;
}
```



### assign cell 

- ```css
  .container {
  	grid-template-areas:
  		"header header header"
  		"   a    main    b   "
  		"   .     .      .   "
  		"footer footer footer";
  }
  ```



## Flex와 Grid

Grid와 Flex 중 어떤걸 사용하는게 좋을까? flex가 훨씬 익숙한 나는 grid를 공부하고 나서 웹페이지들이 다르게 보였다. flex의 특성상 원하는 위치에 원하는 context를 위치시키려면 그 단계까지 흐름을 따라 조정했어야 했는데, grid를 통해 웹에 위치를 먼저 정해두고 원하는 context를 배치시키는 방법이 있다는것을 알게 되었기 때문이다.

웹에서 header나 footer, sidebar처럼 레이아웃의 정확한 위치에 올려두고 싶으면 grid가 권장될 것이고, 여러 context들이 흐름에 따라 자연스럽게 배치되는 경우는 flex가 권장될 것이다. 아직까진 flex가 브라우저 지원이 grid보다 더 다양하게 되고있다. 한쪽으로만 치우쳐져서 사용하지말고 내가 만들려고하는 목적에 맞게 적절히 사용하자.
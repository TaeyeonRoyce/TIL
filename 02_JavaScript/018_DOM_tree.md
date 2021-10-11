# DOM_Tree

## DOM 

DOM(The Document Object Model)은 HTML이나 XML같은 문서의 프로그래밍 인터페이스다. 문서의 구조화된 표현을 제공하여 프로그래밍 언어(JS같은)가 웹페이지를 사용할 수 있게 도와준다. 일종의 문서인 웹페이지를 표현하고, 저장하고, 조작하는 방법을 제공한다.

쉽게 말해 HTML이라는 문서의 `<html>`, `<body>` 와 같은 태그들을 `JavaScript` 같은 프로그래밍 언어가 이용할 수 있는 객체(object)로 만들어 주는 모델(model) 정도

> API (web or XML page) = DOM + JS (scripting language) 



```js
let paragraphs = document.getElementsByTagName("P");
paragraphs[0].style.background = 'red';
//DOM이 HTML의 <p>태그에 접근하여 .style.background 메서드를 통해 조작 가능하게 함

paragraphs[1].innerHTML += "Hello World!";
//JS로 문서객체를 생성하는 동적 생성
```



## DOM 트리

이러한 DOM은 HTML문서를 **태그 트리 구조**로 표현한다.

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>DOM</title>
</head>
<body>
  DOM is Document Object Model
</body>
</html>
```

위 HTML문서를 트리로 표현해보면

 >1. HTML
 >
 >​	1.1 HEAD
 >
 >​		1.1.1 #text \n__
 >
 >​		1.1.2 TITLE
 >
 >​			1.1.2.1 #text DOM
 >
 >​	1.2 #text \n__	
 >
 >​	1.3 BODY
 >
 >​		1.3.1 #text DOM is Document Object Model
 >
 >`*\n = 줄 바꿈, _ = 공백*`

특이한 점은 `#text`이다. 이는 텍스트노드로서 텍스트만 담는 `leaf node`이다. 줄바꿈이나 공백또한 텍스트노드가 될 수 있는데, 2가지 예외사항이 있다.

1. `<head>` 이전의 공백과 새 줄은 무시된다.
2. `</body>` 뒤에 넣어진 콘텐츠는 자동으로 `body` 안쪽으로 옮겨지기 때문에`</body>` 뒤엔 공백이 없다.

그래서 위 코드와 아래코드의 DOM트리는 다르다.

```html
<!DOCTYPE HTML>
<html><head><title>DOM</title></head><body>DOM is Document Object Model</body></html>
```



## DOM 교정

`"Hello World"`만 .html문서에 넣어도 DOM은 부족한 요소들을 자동으로 채워 DOM구조를 완성한다.  => 

```
<!-- DOM -->
<html>
  <head>
  </head>
  <body>
    Hello World
  </body>
</html>
```



DOM은 JS와 밀접하게 연관되어 있지만 분리되어 발전하였다. 때문에 DOM의 구현은 어느 언어에서도 가능하다는 점이 있다.




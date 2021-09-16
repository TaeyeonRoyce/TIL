## Text Overflow

#### 텍스트 줄 바꿈을 처리해보자

#### 텍스트가 담겨있는 박스에 따라 줄바꿈 처리를 다양하게 할 수 있다.



예시 text)

## HTML

```html
<p>Welcome to the Swift community. Together we are working to build a programming language to empower everyone to turn their ideas into apps on any platform. Announced in 2014, the Swift programming language has quickly become one of the fastest growing languages in history. Swift makes it easy to write software that is incredibly fast and safe by design. Our goals for Swift are ambitious: we want to make programming simple things easy, and difficult things possible.</p>
```

## 1. text-overflow 

#### ```text-overflow: ellipsis; ``` 가장 기본적인 방식이며 default value는 clip이다

#### blocked container,  {overflow: hidden;} {white-space: nowrap;} is reqiured!

fade나 원하는 string으로 바꿀 수 있는 api가 있지만 MDN에서 권장하진 않는다.

```CSS
p {
  width: 200px;
  white-space: nowrap;
  overflow: hidden;
}
```

#### Result

><p style="width: 200px;
>  white-space: nowrap;
>  overflow: hidden; text-overflow : ellipsis;"> Welcome to the Swift community. Together we are working to build a programming language to empower everyone to turn their ideas into apps on any platform.</p>

*white-space란 공백문자를 처리하는 방법. 보통은 단어단위(공백)로 줄을 넘기기때문에 설정없이 글을 길게 나열한다면 문단의 크기에 맞게 텍스트가 맞춰지진 않을 수 있다. white-space: nowrap은 default value인 normal (단어단위로 줄바꿈)에서 줄바꿈만 안하는 것이다.

참조:https://developer.mozilla.org/ko/docs/Web/CSS/white-space

## 2. 다른 방식

다른 방법으로 webkit으로 line-clamp을 사용하거나, max-line이라는 프로퍼티를 사용할 수 있는데, 권장되진 않는다고 한다.(https://www.w3.org/TR/css-overflow-3/#propdef-line-clamp)

어차피 줄바꿈을 하려면 담으려는 박스의 크기를 한정하기 때문에(blocked) 충분히 노출되는 줄 수를 조정할 수 있기때문에 굳이 권장되지 않는 프로퍼티를 사용할 필욘 없는 것 같다.

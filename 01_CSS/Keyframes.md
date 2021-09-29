# Keyframe

중간중간의 특정 지점들을 거칠 수 있는 키프레임들을 설정함으로써 CSS 애니메이션 과정의 중간 절차를 제어할 수 있게 한다. 이러한 특성을 이용해서 원하는 애니매이션을 직접 만들 수 있다.

```css
@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
```

사용은 위 코드처럼 `@keyframse`라고 선언한 뒤, 사용할 `animation-name`(여기선 `slidein`)을 정해준다. 그리고 `form` ,`to` 처럼 애니매이션의 시간에 대한 규칙을 포함해야 한다. (0%~100%로도 지점을 정할 수 있다.)

```css
@keyframes identifier {
  0% { top: 0; left: 0; }
  30% { top: 20px; }
  30% { top: 50px; }  /* 키 프레임이 중복지정된 경우에는 가장 최근(top: 50px;)의 키프레임만 유효함*/
  68%, 72% { left: 50px; }
  100% { top: 100px; left: 100%; }
}
```

> `!important`는 키 프레임에서 무시된다.


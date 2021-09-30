# Listing State up

`state` 끌어올리기

데이터의 변경사항을 여러 컴포넌트에 반영하려면, 그 컴포넌트의 공통 조상으로 `state`를 올린후 받는것을 권장. ==> 끌어올리기 라고 함



### 예제로 보기



```jsx
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```

```jsx
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>
          Enter temperature in {scaleNames[scale]}:
        </legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

```jsx
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" /> 
      </div>
    );
  }
}
```

위 코드에서 온도를 입력받으면 ` this.state = temperature: ''`에 저장한 뒤 `<BoilingVerdict />`을 통해 끓는지 안끓는지 체크를 해주는 기능을 가진다. 

`scale`에 따라 두가지 입력창이 존재할 텐데, 이렇게 하면 둘 중 하나에 온도를 입력하더라도 다른 하나는 갱신되지 않는 문제가 생긴다. 이것은 두 입력 필드 간에 동기화 상태를 유지하고자 했던 원래 요구사항과 맞지 않는데, 그 이유는

입력받는 온도가 `<TemperatureInput scale="c" />`에 갇혀 각각의 Element에 독립적으로 저장하고 있기 때문이다.

이렇게 같은 값을 공유하며 동기화가 필요한 두개 이상의 컴포넌트가 있을때 state 끌어올리기가 요구된다.

### State 끌어올리기

우선 독립적으로 사용되는`<TemperatureInput />`의 `state`를 제거하고, 부모인 `<Calculator />`가 전달해주는 `props`를 사용해야한다. 그래서 `const temperature = this.state.temperature` 가 아닌 `const temperature = this.props.temperature`로 변경해준다.

```jsx
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    //state 제거
  }
  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
    //props //onTemperatureChange는 부모인 Calculator로부터 전달 받음
  }
  render() {
    const temperature = this.props.temperature; //props //props.temperature는 부모인 Calculator로부터 전달 받음
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>
          Enter temperature in {scaleNames[scale]}:
        </legend>
        <input value={temperature}
          onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```



```jsx
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {
      temperature: '',
      scale: 'c'
    };  //끌어올린 state => 입력값을 받아서 자식에게 넘겨준다
  }
  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature}); //섭씨 온도 입력 변경 처리
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature}); //화씨 온도 입력 변경 처리
  }
  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;  //단위에 맞게 환산
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature; // tryConvert라는 함수가 있다고 가정
    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />  <!-- props로 temperature를 넘겨줌 -->
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} /> 
      </div>
    );
  }
}
```










# Form



### HTML에서의 form

```html
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

html에서는 폼이 제출되면 새로운 페이지로 이동하는데, `React`에서는 `JS`함수로 폼의 제출을 처리하고 그 데이터에 접근하도록 하는 것을 권장한다. `React`의 state`와 ``setState()`를 활용하자.

이렇게 `JS`로 처리하는 방식을 *`Controlled components`*라고 한다.



### Controlled components

`React state`를 만들어 폼에서 발생하는 입력값을 제어한다.

#### `<Input>`

```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }
  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

` value={this.state.value}`를 통해 폼에서 이루어지는 입력을 받아와서 `handleSubmit()`으로 활용하고, `onChange={this.handleChange}`를 통해 계속해서 업데이트를 시킴



### `<select>`

드롭다운 목록을 만드는 `<select>`도 다룰 수 있는데, 

```html
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```

이 옵션에서 `selected` 키워드는 `React`에서 권장하진 않는다고 한다.

```jsx
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'}; //selected
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }
  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option
              value="grapefruit"> Grapefruit
            </option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

 `this.state = {value: 'coconut'}`을 통해 `selected`를 구현.



`controlled component`가 완벽하진 않기에 추후 '비제어 컴포넌트'나 'Formik'을 통해 폼 제출을 완벽하게 다룰 수 있다.


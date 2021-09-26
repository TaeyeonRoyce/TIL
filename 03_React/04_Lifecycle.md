# Lifecycle

Component가 UI에 rendering 될때 겪는 프로세스가 있다.

- mounting
- updating
- unmounting

## Mounting

`Mounting`은 `Component`가 그려지는 프로세스를 말함. 이때 동작하는 `Lifecycle function`은 

- `constructor()`                    //Creating
- `componentWillMount()`       //Creating
- `render()`                              //Creating
- `componentDidMount() `         //Rendered

순서로 동작한다.



## Updating

`Updating`은 `Component`가 업데이트 되는 프로세스를 말함. 이때 동작하는 `Lifecycle function`은 

- `componentWillReceiveProps()`  //Receving Props
- `shouldComponentUpdate()`         //Receving State
- `componentWillUpdate() `             //Receving State
- `render() `                                      //Receving State
- `componentDidUpdate()`               //Re-Rendered

순서로 동작한다.



## Unmounting

`Unmounting`은 `Component`가 지워져야할때 진행되는 프로세스를 말함. 이때 동작하는 `Lifecycle function`은 

- `componentWillUnmount()` 만 작동한다.



Lifecycle을 활용하여 component가 rendering될때 원하는 시점에 맞게 조작할 수 있으니 위 개념을 잘 알아두자.






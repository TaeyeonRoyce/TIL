# Select Form

### 문제

Movie App을 만들다가 새로운 문제에 직면했다.

```jsx
<Home
  <StickyHeader />
  <Movie /> 
 />
```

이런식의 구조에서 `StickyHeader`에서 변경된 값을 `Home`으로 보내어 그 값에 맞는 `Movie`에 적용해야했다.



### 해결 아이디어

평소에서는 `State`를 상->하 로 `props`를 통해서 전달하면 됬지만, 이번에는 반대로 해야한다. 하 -> 상으로 하위 컴포넌트에 있는 `State`를 상위 컴포넌트에 보내주어야 해서 구글링을 해보았다. 

권장하는 방법인지는 모르겠지만, 구글링 결과로는 상위 컴포넌트에서 하위 컴포넌트로 함수를 하나 전달하고, 하위 컴포넌트에서는 그 함수에 매개변수로 전달하려는 `State` 를 집어넣어 전달하라고 했다.

구현한 코드의 부분만 보면

```jsx

class Home extends React.Component{
  state = {
    sortway : "star" //default값 선언
  }
  getSortWay = (data) => {
    this.setState({sortWay: data}); //하위 컴포넌트로 받아온 data를 sortWay에 갱신
    //setState()가 호출되면서 render를 다시한다.
  }
  
  render(){
    return <StickyHeader sorting={this.getSortWay}/>
  } //props로 getSortWay를 보냄
}


```

```jsx
function StickyHeader(props){
    const [selected, setSelected] = useState("star");
  //state값 선언

    const handleSelect = (e) => {
      setSelected(e.target.value); //state값 갱신
			props.sorting(e.target.value); //상위 컴포넌트로부터 받아온 함수의 매개변수로 입력
    }
    return(
      <select name="choice" onChange={handleSelect} value={selected}> 
        <option value="star" key="star">Star Rating</option>
        <option value="yearfast" key="yearfast">Year-fast</option>
        <option value="yearold" key="yearold">Year-old</option>
      </select>
    )
  //select가 변경될때 handleSelect()
}
```



이런식으로 `<StickyHeader />`에서 받아온 `State`값을 `<Home />`에 전달해 주었다.

전달한 `State`는 `<Movie />`의 정렬 방법이다.



### 정렬 적용하기

```jsx
const sortTypes = {
  star: {
    sortfunc: (a, b) => b.rating - a.rating
  },
  yearfast: {
    sortfunc: (a, b) => b.year - a.year 
  },
  yearold: {
    sortfunc: (a, b) => a.year - b.year
  }
};
// `sortTypes`라는 객체를 생성하고
class Home extends React.Component{
  ...
  {(movies.sort(sortTypes[sortWay].sortfunc))
    .map(movie => (<Movie ... /> ))
	} //sortTypes에 맞는 정렬방식으로 정렬
}
```



`(movies.sort(sortTypes[sortWay].sortfunc))`  살펴보자.

`state: { sortWay : "star"}`이면, 

`(movies.sort(sortTypes[sortWay].sortfunc))` => `(movies.sort((a, b) => b.rating - a.rating))` 가 되어서 정렬을 가능하게 한다.


# Update on Async Rendering
[Update on Async Rendering – React Blog](https://reactjs.org/blog/2018/03/27/update-on-async-rendering.html)


## initializing state
`componentWillMount ` 에서 하지 않고 constructor or property initializer 에서 한다.


## Fetching external data
`componentWillMount` 에서 데이터를 가져오는 것은 문제가 있다.
* 외부 데이터를 사용하지 않는다.
* 여러번 호출될 수 있다.
`componentDidMount` 로 옮기는 것을 추천한다.
`componentWillMount` 에서 호출하는 것이 첫번째 empty rendering 을 피할 수 있다는 오해가 있는데 실제로는 그렇지 않다. React 는 `componentWillMount` 호출 후에 `render` 를 항상 호출한다.


## Adding event listeners (or subscriptions)
Event 를 subscribe 하는 경우에도 위의 경우처럼
`componentDidMount` 에서 `dataSource` 를 subscribe 하고 `componentWillUnmount` 에서 unsubscribe 하도록 한다. 

`dataSource` prop 을 구독하는 것보다 `create-subscription` 을 사용해서 
처리할 수도 있다.


## Updating state based on props
`componentWillReceiveProps` 를 사용해서 새로운 props value 를 업데이트 하는 코드를 작성할 수 있다. 코드 자체로는 문제가 없지만 종종 문제를 일으키기 때문에 이 method 는 deprecated 했다.

`getDerivedStateFromProps` 를 사용해서 업데이트 하도록 변경한다. 이 때,
`prevProps` 도 같이 리턴한다.


## Invoking external callbacks
내부에서 상태가 변화할 때, `componentWillUpdate` 외부 함수를 호출하는 경우가 있을 수 있다. `componentDidUpdate `   에서 호출하면 다른 컴포넌트에서 너무 늦게 전달되지 않을까 생각하는 경우가 있는데 그렇지 않다. 
비동기 모드에서 `componentWillUpdate`  는 unsafe 하다. `componentDidUpdate` 는 업데이트 당 한 번만  호출되는 것이 보장되므로 라이프 사이클을 사용해야 한다.


## Side effects on props change
Side effect 에 의해서 `props` 가 변할 수 있다. `componentWillUpdate` ,
`componentWillReceiveProps` 는 한번 업데이트에 여러번 호출될 수 있기 때문에 피해야 한다. `componentDidUpdate` 는 업데이트마다 한번 호출을 보장한다.


## Fetching external data when props change
역시나 `componentDidUpdate` 와 `getDerivedStateFromProps` 를 사용하는 것을 추천한다.


## Reading DOM properties before an update
목록 내의 스크롤 위치를 유지하기 위해 업데이트 전에 DOM 에서 속성을 읽는 구성 요소의 예
이 상태의 해결책은 `getSnapshotBeforeUpdate` 이다. 이 메소드는 변화가 일어나기 전에 호출 된다. ( DOM 이 업데이트 되기 전에 ) 그리고 호출 직 후에 `componentDidUpdate` 가 발생한다.



# Component LifeCycle API
## componentWillMount
컴포넌트가 화면에 나타나게 됐을 때 호출된다.
D3, masonry 처럼 DOM 을 사용해야하는 외부 라이브러리 연동을 하거나,
해당 컴포넌트에서 필요로하는 데이터를 요청하기 위해 axios, fetch 등을 통하여 ajax 요청을 하거나,
DOM 의 속성을 읽거나 직접 변경하는 작업을 진행합니다.
## componentWillReceiveProps
새로운 Props를 받게 될 때,
## getDerivedStateFromProps
v16.3 이후에 만들어진 라이프사이클 API, props 로 받아온 값을 state 로 동기화 하는 작업을 하는 경우
## shouldComponentUpdate
render 함수 호출 여부
## componentWillUpdate
애니메이션 효과를 초기화하거나 이벤트 리스너를 없애는 작업을 한다.
이 함수 이후에 render 를 호출한다.
## componentWillUnmount
컴포넌트가 더 이상 필요 없을 때
## componentDidUpdate
render 호출 이후에 호출 됨

[lifecycle](https://velopert.com/3631)

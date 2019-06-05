# React API's
render, state, props, context, lifecycle events
stateful api : render, state, lifecycle events
stateless api : render, props, context

`Component patterns` are best use practices and were first introduced to split 
the data or logic layer and the UI or presentational layer

# Component patterns
Common component patterns are
- Container
- Presentational
- Higher order components (HOC's)
- Render callback

## Container 
> A container does data fetching and then renders its corresponding sub-component - Jason Bonta
Containers are your data or logic layer and utilize stateful API's.
Using lifecycle events, you can connect to a state management store such as Redux or Flux, and
pass down data and callbacks as props to children components.

## Presentational
Presentational components utilize props, render, and context (stateless API's)
and can be the syntactically-pretty functional, stateless component
Presentational components receive data and callbacks from props only, which
can be provied by its container or parent component.

## Higher order components (HOC's)
A higher order component is a function that takes a component as an argument and returns a new component.

This is a powerful pattern for providing fetching and data to any number of components and
can be used for reusing component logic. Think react-router-v4 and Redux. With `react-router-v4`, you can use `withRouter()`
to inherit methods passed as props to your component. And with Redux, you have access to actions passed as props
when you connect({})().

# Components
## statless component
재사용성이 굉장히 높은 컴포넌트를 작성할 수 있게 한다. 컴포넌트를 완전한 함수로 정의한다는 점이 특징
```js
const Button = (props) => (
  <button type="button" className={props.className} />
)
// destructuring assignment
const Button = ({ className }) => (
  <button type="button" className={className} />
)
// es6 spread synctax, rest parameters
const Button = ({ className, ...remainProps }) => (
  <button type="button" className={className} {...remainProps} />
)
```



# ref
[react-component-patterns](https://medium.com/teamsubchannel/react-component-patterns-e7fb75be7bb0)
- React 생태계의 슈퍼스타 `Dan Abramov`이 Smart & Dumb 컴포넌트란 이름으로 제시하였습니다. 
[react-basic-components](https://medium.com/little-big-programming/react의-기본-컴포넌트를-알아보자-92c923011818)
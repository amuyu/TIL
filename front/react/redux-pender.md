

# penderReducer
요청을 관리하는 reducer 로 pending, success, failure 상태를 가진다.

```js
export const getPost = createAction(GET_POST, getPostAPI);


const initialState = {
    // 요청이 진행중인지, 에러가 났는지의 여부는 더 이상 직접 관리 할 필요가 없어집니다. penderReducer 가 담당하기 때문이죠
    data: {
        title: '',
        body: ''
    }
}

export default handleActions({
    ...pender({
        type: GET_POST, // type 이 주어지면, 이 type 에 접미사를 붙인 액션핸들러들이 담긴 객체를 생성합니다.
        /*
            요청중 / 실패 했을 때 추가적으로 해야 할 작업이 있다면 이렇게 onPending 과 onFailure 를 추가해주면됩니다.
            onPending: (state, action) => state,
            onFailure: (state, action) => state
        */
        onSuccess: (state, action) => { // 성공했을때 해야 할 작업이 따로 없으면 이 함수 또한 생략해도 됩니다.
            const { title, body } = action.payload.data;
            return {
                data: {
                    title, 
                    body
                }
            }
        }
        // 함수가 생략됐을때 기본 값으론 (state, action) => state 가 설정됩니다 (state 를 그대로 반환한다는 것이죠)
    })
}, initialState);
```

`applyPenders` 를 호출해서
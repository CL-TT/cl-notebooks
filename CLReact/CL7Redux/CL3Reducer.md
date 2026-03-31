# Reducer

## 1. Reducer的概念以及要注意的地方

1. **reducer就是一个用于改变仓库数据的函数**
2. 一个数据仓库，有且只有一个reducer，并且通常情况下一个工程只有一个仓库，因此一个系统只有一个reducer（当然后面可以分化）
3. reducer被调用的时机， **当创建store的时候会调用一次reducer，被称为初始化， 分发action时，会调用reducer**
4. **reducer必须是一个没有副作用的纯函数， 不能改变原有的状态，必须返回新的状态， 不能有异步操作**





## 2. 分化reducer然后合并,并且使用

1. 比如说数据仓库有两种数据，用户数据， 登录状态

```js
export const SETLOGIN = 'setlogin';

// 设置登录状态，布尔值，true为登录， false为没有登录
export function getSetLoginAction(payload){
    return {
        type: SETLOGIN,
        payload
    }
} 


export const ADDUSER = 'adduser';
export const DELUSER = 'deluser';
export const UPDATEUSER = 'updateuser';

export function getAddUserAction(payload){
    return {
        type: ADDUSER,
        payload
    }
}

export function getDelUser(id){
    return {
        type: DELUSER,
        payload: id
    }
}

export function getUpdateUserAction(id, user){
    return {
        type: UPDATEUSER,
        payload: {
            id,
            user
        }
    }
}
```

```js
// 登录状态的reducer

import { SETLOGIN } from ''

export default function(state = false, action){
    switch(action.type){
        case SETLOGIN:
            return action.payload
        default: 
            return state
    }
}


// 用户数据的reducer

import { ADDUSER, DELUSER, UPDATEUSER } from ''

export default function(state = [], action){
    switch(action.type){
        case ADDUSER: 
            return [ ...state, action.payload ]
        case DELUSER:
            return state.filter(i => i.id !== action.payload)
        case UPDATEUSER: 
            return state.map(i => i.id === action.payload.id? action.payload.user : i)
        default: 
            return state
    }
}
```

合并reducer

```js
import setLoginReducer from ''
import setUserReducer from ''

export default function(state, action){
    const newState = {
        loginStatus: setLoginReducer(state.loginStatus, action),
        users: setUserReducer(state.users, action)
    }
}

// redux中有一个专门的方法给你合并reducer
import { combineReducers } from 'redux'

export default combinereducers({
    loginStatus: setLoginReducer,
    users: setUsersReducer
})
```

使用

```js
import reducer from '';

import { createStore } from '';

import { getSetLoginAction } from ''

const store = createStore(reducer);

store.dispatch(getSetLoginAction(true));
```


# Redux数据状态管理

## 1. Redux的核心概念 (最原始的方案)

npm i redux

使用的缺点还是需要从顶层组件一个一个的往下传

![](D:\CL武林秘籍\画图\redux1.png)

```jsx
1. 视图views通过分发action到仓库

2. 然后仓库把action提交给reducer

3. reducer,里面真正能改变仓库数据的，然后把改变后的新数据返回给仓库

代码演示
redux/actionTypes   action类型文件

export const ADD = 'ADD'


redux/actionsCreator    action对象创建文件

export const addList = (item) => ({
    type: ADD,
    payload: item
})

redux/reducer
state是仓库初始的数据
export default function (state = [], action) {
    switch(action.type) {
         case: ADD
            return [...state, action.payload ]
         case: default
            return state
    }
}

redux/store

import { createStore } from 'redux'
import reducer from ''

export const store = createStore(reducer)

挂载到根组件上, 在函数组件的props上会有store这个属性
<APP store={ store } />
    
props.store.dispatch(addList({}))   // 分发action
props.store.state   // 要使用仓库里面的数据
```



## 2. React-redux 和  @reduxjs/toolkit  (最佳实践)

npm i react-redux @reduxjs/toolkit

```jsx
redux/store.js  仓库文件

import { configureStore } from '@reduxjs/toolkit'
import userSlice from ''

const store = configureStore({
    reducer: {
        userSlice
    }
})

export default store

redux/userSlice.js  用户切片

import { createSlice } from '@reduxjs/toolkit'

const userSlice = createSlice({
    name: 'userSlice',
    initialState: {
        count: 0
    },
    reducers: {
        add(state, { payload }) {
            state.count = state.count + payload
        }
    }
})
export const { add } = userSlice.actions
export default userSlice.reducer

仓库的使用

import { Provider } from 'react-redux'

Provider提供上下文
<Provider store={ store }>
    <App />
</Provider>
import { useSelector, useDispatch } from 'react-redux'
import { add } from ''

function App() {
    // 获取仓库数据
    const { count } = useSelector(state => state.userSlice)
    
    const dispatch = useDispatch()
    
    // 改变仓库数据
    dispatch(add(1))
    
    // 如果是异步操作
    dispatch(getListAsync())
}


如果有异步函数需要请求服务器的内容

import { createAsyncThunk } from '@reduxjs/toolkit'

export const getListAsync = createAsyncThunk('userSlice/getListAsync', async (payload, thunkApi) => {
    const res = await getList()
    
    //派发更新
    thunkApi.dispatch(...)
})
```


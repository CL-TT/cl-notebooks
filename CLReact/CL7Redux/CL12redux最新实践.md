# redux最新实践

## 1. 安装两个库

npm i react-redux  @reduxjs/toolkit



## 2. 最佳实践

原来的redux有store,  action,  actionType, reducer四个文件，  现在有了安装的两个依赖，可以简化成两个文件

```react
store.js  创建仓库

import { configureStore } from '@reduxjs/toolkit'
import LoginUserSlice from ''

const store = configureStore({
    reducer: {
        loginUser: LoginUserSlice
    }
})

export default store
```

```react
loginUserSlice.js   每一个模块的切片    比如说这是登录模块

import { createSlice } from '@reduxjs/toolkit'

// 创建一个切片
const loginUserSlice =  createSlice({
    name: 'loginUser',   // 切片的命名空间
    
    // 初始状态数据
    initialState: {
        isLogin: false
    },
    
    // 具体的操作
    reducers: {
        // state: 仓库的状态数据
        // payload：参数
        // 可以直接对state进行修改
        add: (state, { payload }) => {
            state.num = state.num + 1
        },
        
        sub: () => {
            
        }
    }
})

export const { add, sub } = loginUserSlice.actions

export default loginUserSlice.reducer
```

```react
index.js   使用仓库数据

import { Provider } from 'react-redux'
import store from ''

<Provider store={ store }>
    <App />
</Provider>


// 如果仓库变化，就会执行
store.subscribe(() => {
    render()
})
```

```react
在子组件中怎么拿仓库数据, 和改变仓库数据

使用react-redux提供的hook

import { useSelector, useDispatch } from 'react-redux'
import { add, sub } from ''

function index() {
    // 解构出登录模块的数据
    const { } = useSelector(state => state.loginUser)
    
    // 如果想改变仓库里面的数据
    const dispatch = useDispatch()
    
    //传入一个action给dispatch
    displatch(add())
}
```

```react
如果仓库中有异步操作, createAsyncThunk

import { createSlice, createAsyncThunk } from '@reduxjs/toolkit'

/**
* 1. 第一个参数是名称，一个名称标识
* 2. 第二个参数是一个异步参数
*  2.1 函数的第一个参数是要传一些额外的参数
*  2.2 函数的第二个参数是，集成了dispatch, getState等的一个对象
**/
export const getStuList = createAsyncThunk('stu/getStuList', async (_, thunkApi) => {
    // 发送ajax请求
    const res = await getStuList()
    
    // 拿到数据之后要改变仓库的数据, 要分发action
    thunkApi.dispatch(initStuList(res.data))
    
    // 处理异步的数据还有其他的方式, 这是第二种方式
    return res.data
})

const stuSlice = createSlice({
    name: 'stu',
    
    initialState: {
        stuList: []
    },
    
    reducers: {
        initStuList: (state, { payload }) => {
            state.stuList = payload
        }
    },
    
    // 专门用来处理异步的数据
    extraReducers: {
        [getStuList:fulfilled]: (state, { payload }) => {
            state.stuList = payload
        }
    }
})

export const { initStuList } = stuSlice.actions


// 在组件中的使用

import { useDispatch } from 'react-redux'
import { useEffect } from 'react'
import { getStuList } from ''   // 引入那个可以异步操作的

function index() {
    const dispatch = useDispatch()
    
    useeffect(() => {
        dispatch(getStuList())
    }, [])
}
```


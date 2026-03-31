# redux-actions(是库不是中间件)

1. 这个库就是为了简化创建action-type, action, 和reducer的过程

## 1. createAction

1. 创建单个的action创建函数

```js
import { createAction } from 'redux-actions';

export const ADDNUM = 'ADD_NUM';

// 第一个参数是action的类型， 第二个参数是一个函数， 返回值作为payload
export const getAddNumAction = createAction(ADDNUM, num => num);
```



## 2. createActions

1. 创建多个action创建函数，并且不用单独写action类型

```js
import { createActions } from 'redux-actions';

const actions = createActions({
    ADDNUM: null,
    SUBNUM: null,
    SETNUM: num => num
})

// 结构出来的就是action创建函数， addnum.toString() 就会得到action类型字符串
export const { addnum, subnum, setnum } = actions;
```



## 3. handleAction和handleActions

```js
import { handleAction, handleActions } from 'redux-actions';

// 只能处理单个action
const numReducer = handleAction(ADDNUM, (state, { type, payload }) => {
    
}, 默认值)

// 处理多个actions
const numReducer = handleActions({
    [addnum]: (state) => {
        
    },
    
    [subnum]: (state) => {
        
    },
    
    [setnum]: (state, { payload }) => {
        
    }
}, 默认值)
```



## 4. combineActions

1. 为了整合多个action指向同一个reducer的操作

```js
import { combineActions, handleActions } from 'redux-actions';

const all = combineActions(addnum, subnum, setnum);

const numReducer = handleActions({
    [all]: (state, { payload }) => {
        return state + payload
    }
}, 5)

只要触发addsum， subnum, setnum其中的任何一个， 都会触发all action
```


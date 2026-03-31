# Redux中间件之副作用中间件

## 1. redux-thunk

1. **thunk允许action是一个带有副作用的函数，当action是一个函数的时候，他不会向后面移交了，会立马执行这个函数**

```js
//手写redux-thunk


const createThunkMiddlewear = (extra) => {
    return store => next => action => {
        if(type action === 'function'){
            //这个函数有三个参数， 第一个是dispatch， 第二个是getState, 第三个是额外参数
            return action(store.dispatch, store.getState, extra);
        }
        return next(action);
    }
}

const thunk = createThunkMiddlewear();

thunk.withExtraArgument = createThunkMiddlewear


```





## 2. redux-promise

1. redux-promise也是在action阶段允许让你有副作用操作
2. 如果action是一个promise，则会等待promise完成之后，将完成的结果作为action触发，如果action不是一个promise，则判断payload是否是一个promise，如果是等待promise完成，然后将得到的结果作为payload的值

```js
const SETLOADING = 'setloading';

function getLoadingAction(payload){
    return {
        type: SETLOADING,
        payload
    }
}


// 第一种写法
function getStuAction(){
    return new Promise(resolve => {
        const action = getLoadingAction(true);
        resolve(action);  // 就相当于dispatch(getLoadingAction(true))
    })
}

// 第二种写法
async function getStuAction(){
    // 一些副作用
    return getLoadingAction(true);
} 
```

```js
// 手写redux-promise

// 标准的flux action的条件
// 1. action必须是一个平面对象 plain-object
// 2. action.type必须是一个字符串
// 3. action的属性不能包含其他非标准的属性， 标准属性[ 'type', 'payload', 'error', 'meta' ]

import { isPlainObject, isString } from 'lodash';
import isPromise from 'is-promise'

export ({ dispatch, getState }) => next => action => {
    //先要判断action是不是一个标准的flux action
    if(!isFSA(action)){
        //如果不是一个标准的action， 在判断是否是一个promise
        
        //如果是一个promise, 就等待执行这个promise，如果不是一个promise就next
        return isPromise(action)? action.then(ac => dispatch(ac)) :  next(action)
    }
    // 是标准的就需要判断payload属性是否是一个promise
    return isPromise(action.payload)? 
        action.payload.
    then(ac => dispatch({ ...action, payload: ac })).
    catch(error => dispatch({ ...action, payload: error, error: true })) : next(action)
}

/**
* 判断action是否是一个标准的action
**/
function isFSA(action){
    return isPlainObject(action) && isString(action.type) && Object.keys(action).every(key => ['type', 'payload', 'error', 'meta'].includes(key))
}
```


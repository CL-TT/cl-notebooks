# Action

## 1. Action的概念以及要注意的地方

1. **action必须是一个平面对象**(不能以class的方式创建出来)， 它的_proto_指向的是Object.prototype

2. **action中必须要有type属性**，type属性的值的类型并没有做什么要求，所以不一定是字符串

3. 一般会将action的类型抽离出来，形成一个单独的文件

4. 通常为了方便传递action，通常会使用action创建函数来创建action

   #1. action创建函数应该为无副作用的纯函数

   #2. 纯函数就是不能以任何形式改动参数

   #3. 不可以有异步

   #4. 不可以对外部的环境造成影响

5. 如果需要传值的话，做好用payload属性， 但没有做强制要求



## 2. 最佳实践

1. action类型文件

```js
export const ADD = 'add';

export const SUB = 'SUB';
```

2. 创建action的函数文件

```js
import * as actionType from '';

export function getAddAction(){
    return {
        type: actionType.ADD
    }
}

export function getSubAction(){
    return {
        type: actionType.SUB
    }
}
```

3. 使用

```js
import { createStore } from 'redux';

import * as actionsFun from '';

function reducer(state, action){
    
}

const store = createStore(reducer, 10);

store.dispatch(actionsFun.getAddAction());


// redux中还有一个 bindActionCreators, 它是一个和函数， 第一个参数是创建action的函数的对象集合， 第二个参数是store中的dispatch

const bindAction = bindActionCreators(actionsFun, store.dispatch);

bindAction.getAddAction();
```


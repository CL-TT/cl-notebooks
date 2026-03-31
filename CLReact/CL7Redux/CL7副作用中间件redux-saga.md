# 副作用中间件redux-saga

## 1. redux-saga概念以及使用

1. 有一个saga任务， 他是一个生成器函数， 如果里面的yield了一个普通的数据，saga不做任何处理，仅仅将数据传递给yield表达式(把得到的数据放到next的参数中)， 因此在saga中，yield一个普通的数据没什么意义

```js
import createSagaMiddlewear from 'redux-saga';
import { createStore, applyMiddlewear } from 'redux';

/**
* saga任务
**/
function *saga(){
    
}

// 创建一个saga中间件
const sagaMiddlewear = createSagaMiddlewear()

// 应用saga中间件
const stroe = createStore(reducer, applyMiddlewear(sagaMiddlewear));


// 启动saga任务
sagaMiddlewear.run(saga);
```

2. 流程：**最开始的时候启动一个saga任务（就是一个生成器函数），saga为任务提供了大量功能供其使用， 这些功能以指令的形式出现， 并且出现在yield的位置， 因此可以被saga中间件控制他的执行** 

```js
// 最佳实践， 会把不同的saga任务分开，如下

// 这是控制count数据的saga
export default function *(){
    
}


// 这是控制user数据的saga
export default function *(){
    
}


// 合并saga的话需要使用  all指令

import { all } from 'rudex-saga/effects';
import userSagaTask from '';
import countSagaTask from '';

export default function *(){
    // 一定要调用函数 
    yield all([ userSagaTask(), countSagaTask() ])
}

```



## 2. saga指令

### 1. take指令

1. take指令【阻塞】： 监听某个action，如果action发生了， 则会进行下一步处理， take指令仅监听一次， yield得到的是完整的action对象
2. **如果这个action没有触发， 那就就会阻塞任务**，这个就不会结束，不会往下运行
3. **一旦saga任务运行完成， saga中间件一定结束**

```js
import { take } from 'redux-saga/effects';

function *(){
    console.log('开始运行');
    
    const action = yield take(getAddAction);
    
    console.log(action) // 得到的是一个完成的action
}
```

### 2. all指令

1. all指令【阻塞】： 该函数传入一个数组，数组中放入生成器， saga会等待所有的生成器全部完成之后才会进一步处理

### 3. takeEvery指令

1. takeEvery指令： 不断的监听某个action，当某个action到达之后，运行一个函数,   **传入的是action的操作名称**，**而不是action的创建函数**，**否则在saga任务中就会导致死循环**

```js
import { takeEvery } from 'redux-saga/effects';

function *(){
    yield takeEvery(ADD, subTask)
}

function *subTask(){
    console.log('正在监听subAction');
}

当getSubAction被触发， subTask函数会被运行
```

### 4. delay指令

1. delay指令【阻塞】: 阻塞指定的毫秒数

```js
import { delay } from 'redux-saga/effects';

function *(){
    yield delay(3);   // 相当于在这写了一个setTimeout
}
```



### 5. put指令

1. put指令：用于重新触发action， 相当于dispatch一个action

```js
import { put } from 'redux-saga/effects';

function *(){
    yield put(getAddAction)
}
```



### 6. call指令

1. call指令【可能阻塞】：用于副作用（通常是异步）函数调用

```js
import { call } from 'redux-saga/effects';

function *(){
    yield call(getStudents, 参数1， 参数2)
    
    yield call([this, getStudents], 参数1， 参数2)
}
```



### 7. apply指令

1. apply指令【可能阻塞】：用于副作用（通常是异步）函数调用

```js
import { apply } from 'redux-saga/effects';

function *(){
    yield call(getStudents, [参数1， 参数2])
    
    yield call(this, getStudents, [参数1， 参数2])
}
```



### 8. select指令

1. select指令【可能阻塞】：用于得到当前仓库中的数据

### 9. cps指令

1. cps指令【可能阻塞】用于调用那些传统的回调方式的异步函数

### 10. fork指令

1. fork指令：用于开启一个新的任务， 该任务不会阻塞，该函数需要传递一个生成器函数，fork返回了一个对象，类型为Task

### 11. cancel指令

1. 用于取消一个或多个任务，实际上，取消的实现原理，就是利用generator.return, cancel可以不传递参数， 如果不传递参数，则取消当前任务线

### 12. takeLastest指令

1. 功能和takeEvery一致， 只不过， 会自动取消之前的开启的任务

### 13. cancelled指令

1. 判断当前任务线是否被取消掉

### 14. race指令

1. race指令【阻塞】：可以传递多个指令， 当其中任何一个指令结束之后，会直接结束，与promise.race类似
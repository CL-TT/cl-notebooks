# store

```js
import { createStore } from 'redux';

const store = createStore(reducer);

// store中的方法

1. dispatch: 分发一个action

2. getState: 得到仓库中的当前状态

3. replaceReducer: 替换掉当前的reducer

4. subscribe: 注册一个监听器，监听器是一个无参函数， 分发一个action之后，会运行注册的监听器，该函数会返回一个函数，用于取消监听
```


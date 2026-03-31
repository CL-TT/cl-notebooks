# 事件总线插件mitt的使用

```js
1. 安装

npm i mitt

2. 在utils创建eventBus.js

import mitt from 'mitt'

const emitter = mitt()

export default emitter

3. 使用

const onScroll = () => {
    
}

emitter.on('onScroll', onScroll)  // 监听这个事件的触发



// 触发这个事件

emitter.emit('onScroll')
```


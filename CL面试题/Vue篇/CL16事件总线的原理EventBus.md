# 事件总线的原理EventBus

## 1. 事件总线的应用场景

当不是父子关系的组件想要通信时，就可以通过事件总线



## 2. 事件总线的原理

Vue 的事件总线的原理很简单，他实际上就是一个全局的事件中心， 每一个组件都可以通过$emit方法触发这个事件，其他组件可以通过$on方法监听这个事件，当事件被触发时，所有监听这个事件的组件都会收到通知，那么就可以拿到传递过来的数据

```js
事件总线需要做的事情

1. 提供监听某个事件的接口

2. 提供取消监听的接口

3. 触发事件的接口(可传递数据)

4. 触发事件后会自动通知监听者
```



## 3. 事件总线的实现方式

```js
1. 直接导出Vue实例

export default new Vue({})


2. 自己实现

const handlers = {}

export default {
    $on: (eventName, handler) => {
        if (handlers[eventName]) {
            handlers[eventName].add(handler)
        } else {
            const set = new Set()
            
            set.add(handler)
            
            handlers[eventName] = set
        }
    }
    
    $emit: (eventName, ...args) => {
        if (!handlers[eventName]) return
        
        for (let h of handlers[eventName]) {
            h(...args)
        }
    }

    $off: (eventName, handler) => {
        if (!handlers[eventName]) return
        
        handlers[eventName].delete(handler)
    }
}
```


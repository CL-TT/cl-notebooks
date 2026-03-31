# 跨标签页通信

## 1. BroadcastChannel

1. 可以实现多个浏览器上下文（标签页，iframe，窗口）之间的消息广播
2. 使用简单，API友好，支持多标签页，iframe和窗口之间的即时通信，**不受同源限制**
3. 浏览器支持情况较新，不支持旧版本浏览器

```js
const channel = new BroadcastChannel('channel')   // 建立一个广播通道

// 监听通道的消息
channel.onmessage = () => {
    
}

// 发送消息
channel.postMessage()
```



## 2. Service Worker

1. 可以在后台脚本中实现消息的接收和发送
2. 可以实现更复杂的逻辑，离线处理，网络请求拦截等
3. 缺点：实现较为复杂，需要https的支持，初次设置和调试成本较高

```js
vue 项目

public目录下新建sw.js

self.addEventListener('message', async e => {
    const clients = self.clients.matchAll()
    
    clients.forEach(client => {
        client.postMessage(e.data)
    })
})


1.vue

navigator.serviceWorker.registor('sw.js')

navigator.serviceWorker.controller.postMessage({})


2.vue

navigator.serviceWorker.registor('sw.js')

navigator.serviceWorker.onmessage = (e) => {
    console.log(e.data)
}

```



## 3. localstorage+storage事件

1. 特点：就是通过localstorage存储数据，利用storage事件在不同的标签页之间同步数据变化
2. 优点：浏览器兼容好，使用简单
3. 缺点：只能在同源页面之间通信，且数据只能是字符串类型，storage事件仅在不同的标签页间有效，同一标签页的同域页面间无法触发

```js
1.vue

localStorage.value = ''

2.vue

window.onstorage = (e) => {
    
}
```



## 4. postMessage

1. 特点：通过window.postMessage在不同的窗口或iframe之间发送消息
2. 适用于窗口之间或iframe之间的通信，如果是标签页之间的通信需要通过父窗口代理

```js
1.vue

let openr = null

openr = window.open('2.vue')

openr.postMessage({}, '*')


2.vue

window.addEventListener('message', (e) => {
    
})
```


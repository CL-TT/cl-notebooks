# WebSocket原理

## 1.演变过程

```js
1. socket

客户端连接服务器端， 基于tcp/ip协议， 三次握手，  建立连接通道
然后自由通信，消息格式不做要求
客户端或者服务器端中断，连接通道销毁

2. http

客户端连接服务器， 基于tcp/ip协议，  三次握手，  建立连接通道
然后客户端主动发送请求， 服务器接到请求在响应信息给客户端， 消息格式是有要求的
客户端或者服务器端中断，连接通道销毁
```



## 2.数据实时性的问题

```js
1. 在很多时候，很多场景，对数据的要求都是需要实时性的，比如聊天，股票等

2. 怎么解决数据实时性的问题呢？

http的解决方案

@1. 轮询： 会在客户端写一段程序，隔一段时间就会向服务器做出询问，你的数据有没有发生改变，发生改变就让服务器把新的数据再发给我，这样做的缺点就是会产生许多无效的询问

@2. 长连接： connection: keep-alive, 请求的时候会在请求头主动带这个字段，会让服务器在一段时间内一直保持连接状态
```



## 3.websocket详解

#### 1.websocket要解决什么问题呢？

websocket专门用于解决数据实时性传输的问题

#### 2.流程

客户端连接服务器，基于tcp/ip协议， 三次握手，建立连接通道

**客户端会先向服务器端发送一个http格式的消息，这是去向服务器询问你接受websocket这种通信方式吗，如果服务器接收，那么服务器也会向客户端发送一个http格式的消息，这个过程被称为http握手**

然后客户端和服务器自由通信，但通信的消息格式要按照websocket的要求来

客户端或者服务器中断，连接通道销毁



```js
服务端的握手响应

在websocket的HTTP握手响应阶段， 服务器响应头应包含如下内容

Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: [key]

其中Sec-WebSocket-Accept的值来自于以下算法

base64(sha1(Sec-webSocket-Key) + "258EAFA5-E914-47DA-95CA-C5AB0DC85B11")

在node中使用以下代码获得
const crypto = require('crypto');
const hash = crypto.createHash('sha1');
hash.update(requestKey + "258EAFA5-E914-47DA-95CA-C5AB0DC85B11");
const key = hash.digest('base64');

其中requestKey来自于请求头中的'Sec-WebSocket-Key'
```



## 4.第三方库socket.io

npm i socket.io

```js
1. 在服务端使用socket.io

const express = require('express');
const http = require('http');
const path = require('path');
const socketIO = require('socket.io');

const app = express();
app.use(express.static(path.resolve(__dirname, 'public')));
const server = http.createServer(app);

const io = socketIO(server);

//新的用户建立连接
io.on('connection', socket => {
    console.log('有新的用户连接过来了');
    
    //监听用户发送消息
    socket.on('msg', chunk => {
        console.log(chunk.toString('utf-8'));
    })
    
    setInterval(() => {
        socket.emit('test', 'server give message to client');
    }, 2000)
    
    //监听断开连接
    socket.on('disconnection', () => {
        console.log('断开连接');
    })
})

server.listen(9800, () => {
    console.log('服务器成功启动');
})
```



```js
1. 在客户端使用socket.io 

<script src="https://cdn.bootcdn.net/ajax/libs/socket.io/4.0.1/socket.io.js"></script>

const socket = io.connect();

socket.emit('', '');

socket.on('', () => {
    
})
```


























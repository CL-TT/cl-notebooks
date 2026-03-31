# NET模块

## 1.创建一个客户端

1. 客户端向服务器端发送数据和接收数据都是通过流的操作来完成的， 往流里面写入的内容就是客户端向服务器端发送的消息
2. socket     <==>      tcp/ip协议            =>           远程主机

```nodejs
const net = require('net');

const clientSocket = net.createConnection({
  host: 'duyi.ke.qq.com',   //主机地址
  port: 80,   //端口号
}, () => {
console.log('连接成功');
})

1. 返回一个socket，他是一个特殊的文件， 在node中表现为一个双工流对象，通过向流写入内容发送数据，通过监听流的内容来获取数据


2. 给服务器发送消息，就必须按照http协议来，请求行和请求体之间必须要换行
socket.write(
`GET/HTTP/1.1
 HOST: duyi.ke.qq.com
 Connection: keep-alive
 
 
`
)


3.接收从服务器传过来的消息
socket.on('data', chunk => {
console.log('服务器传过来的消息' + chunk.toString('utf-8'));
})


4.结束连接
socket.end();
```





## 2.创建一个服务器

```node
const net = require('net');

const server = net.createServer();

server.listen(9527);       //端口号9527进行连接

server.on('listening', () => {
  console.log('端口号9527正在监听');
})

server.on('connection', () => {
  console.log('有客户端进行连接');
})
```


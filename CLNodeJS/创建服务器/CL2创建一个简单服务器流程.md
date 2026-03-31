# 创建一个简单服务器流程

## 1. HTTP模块

http这个模块的职责就是帮你创建编写服务器



## 2. 具体流程

```javascript
//1.加载核心模块http
var http = require('http');  

//2.使用http.createServer()创建一个简单的服务器
var server = http.createServer();

//3.服务器提供服务，监听request请求，执行回调函数
//这个回调函数有两个参数request, response
server.on('request', function(request, response){
    response.setHeader('Content-Type', 'text/html;charset=UTF-8');   //解决中文乱码
    
    response.end('响应数据');     //服务器给浏览器返回的响应数据
    //response的响应内容只能是二进制或者字符串
    //如果是对象就用JSON.stringify(obj)
})

//4.绑定端口号，启动服务器
server.listen(3000, function(){
    console.log('成功启动服务器');
})
```


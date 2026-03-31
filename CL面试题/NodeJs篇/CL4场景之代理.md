# 代理

## 1.代理的场景以及原理

![](D:\CL武林秘籍\画图\代理服务器.jpg)

1. 客户端发送请求给代理服务器，然后代理服务器在转给数据服务器
2. 数据服务器把数据响应给代理服务器，代理服务器再把数据转给客户端



## 2.手写代理中间件

```js
const http = require('http');

module.exports = (req, res, next){
    const proxyPath = '/data';
    
    if(!req.path.startsWith(proxyPath)){
        next();
        return;
    }
    
    //创建一个代理请求对象
    const proxyRequest = http.request({
        host: '',
        port: '',
        method: req.method,
        headers: req.headers
    }, response => {
        //响应状态码
        res.status(response.statusCode);
        //响应头
        for(let prop in response.headers){
            res.setHeader(prop, response.headers[prop]);
        }
        //响应体同样以管道的方式给客户端
        response.pipe(res);
    })
    
    //把请求体通过管道的方式给代理请求对象
    req.pipe(proxyRequest);
}
```



## 3.http-proxy-middleware

```js
1. 官方的代理中间件  npm i http-proxy-middleware

const { createProxyMiddleware } = require('http-proxy-middleware');

const proxyPath = '/data';

module.exports = createProxyMiddleware(proxyPath, {
    target: 'http://yuanjin.tech:5100',
    pathRewrite: (path, req) => {
        return path.substr(proxyPath.length);
    }
})
```


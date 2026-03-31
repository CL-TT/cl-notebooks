# 登录和验证详解

## 1.安装cookie-parser

1. 安装 npm i cookie-parser
2. 使用这个中间件 app.use(require('cookie-parser'))
3. 在加入之后，会在req对象中注入cookies属性，用于获取所有请求传递过来的cookie
4. 在加入后，会在res对象中注入cookie方法，用于设置cookie



## 2.客户端表单登录以及其他API的请求

```js
1. 客户端通过表单填写完账号，密码，发送请求到服务器
2. 在登录成功并拿到登录凭证后，在其他的请求中浏览器会自动携带服务器发送过来的cookie
```



## 3.服务器对cookie的处理

```js
1. 服务器接收到客户端的请求
2. 先用cookie-parser中间件，对每一次请求进行处理，对req,res对象分别注入cookies属性和cookie方法，方便对cookie进行操作
3. 服务器先查看是否有这个用户，和数据库匹配是否有改账户和密码，如果有的话服务器会在响应头里设置cookie属性
if(result){
    //登录成功的状态, 在响应头里设置cookie
    res.cookie('token', value, {
        httponly: true,
        path: '/',
        domain: 'localhost',
        maxAge: 3600 * 24
    })
    
    //这个是为了其他终端的使用
    res.header('authorization', value);
}

4. 做一个处理token的中间件，处理其他请求
const regexpPath = require('path-to-regexp');  //对路径处理的库

function handleToken(res, req, next){
    const whiteList = [];  //免登录验证的白名单
    
    whiteList.filter(api => {
        const reg = regexpPath(api.path);
        return api.method == req.method && reg.test(req.path);
    })
    
    if(whiteList.length == 0){
        //证明这一次请求不在登录白名单中，需要验证
        let token = req.cookies.token;
        if(!token){
            token = req.header.authorization
        }
        if(!token){
            //未登录状态
        }else{
            //登录状态，直接next
            next();
            return;
        }
    }else{
        //在名单中
        next();
    }
}
```



## 4.对token数据进行加密

```js
1. 如果不对token的值就行加密的话，那么在浏览器端就可以伪造token, 这样同样可以绕过登录访问数据。
```


# Session的基本使用

## 1.Session的特点

1. session是保存在服务器端的
2. 正因为他保存在服务器端，优点显而易见：他的存储量大，可以是任何的存储格式，存储安全，数据不容易被获取和泄漏，数据难以被篡改
3. 缺点：他会占用服务器的资源，增加了服务器端的压力



## 2.Session的原理

1. 只要浏览器那边有请求过来，服务器就会给他返回一个sessionId: value， 然后下一次浏览器请求就会携带这个sessionId

2. 服务器会在内部维护一个表格，这个表格可能会存储在内存中或数据库中(服务器被重启，内存中的数据就会被刷新)，表格的格式如下：

3. 登录请求时，服务器会给这个sessionId（uuid,全球唯一id），一个value值，并把这个sessionId通过cookie的方式返回给浏览器

4. 等下一次请求需要验证权限时，会携带这个sessionId， 服务器会根据这个sessionId到内部表格中查看这个sessionId是否有值，如果有值那么他肯定登录过了，没有值那么就还没有登录， 以此来实现登录权限验证

5. 服务器是不知道浏览器什么时候关闭的，所以服务器认为你浏览器长时间没有附带sessionId来请求我的话， 那么我就会把这个表格中的sessionId销毁掉

   ![](D:\CL武林秘籍\画图\session原理示意图.jpg)



## 3.session的使用

```js
const session = require('express-session');

app.use(session(
    secret: '',    //需要加密的
));

1. 登录成功时
req.session.loginUser = value

2. 写一个处理session的中间件，判断
if(req.session['loginUser']){
    //有值证明登录过了
    next();
}
```


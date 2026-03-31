# Express的基本使用

## 1.基本使用

```javascript
//1. 安装express  npm install express --save

//2. 引入  var express = require('express');

//3. var app = express();    //相当于执行 http.createServer();

//4. app.get('/', function(req, res){    //监听请求
//      res.send('');    //响应的数据
//   })

//5. 监听端口
//   app.listen(3000, function(){console.log('服务器成功启动')})

//6. app.use('/public/', express.static('./public/'));  //可以访问的静态资源
```



## 2.nodemon的使用

1. 它的功能相当于热部署， 就是不用自己手动重新加载代码， 会给你自动重载代码， 非常方便

2. npm i -D nodemon

3. npx nodemon index.js

4. 基本配置

   ```javascript
   //1. 在package.json中
   "scripts": {
       "start": "nodemon -x npm run server",
       "server": "node index"
   }
   
   
   //2. nodemon.json文件中
   //我们不希望什么东西已修改他都要重新加载代码， 所以我们需要配置文件，来忽略某些文件的修改
   {
       "watch": ["*.js", "*.json"],
       "ignore": ["package*.json", "node_modules", "public", "nodemon.json"]
   }
   ```

   
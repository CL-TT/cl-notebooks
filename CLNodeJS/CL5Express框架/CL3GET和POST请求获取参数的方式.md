# GET和POST请求获取参数的方式

## 1. GET请求获取参数的方式

**var obj = req.query**

```javascript
var express = require('express');

var app = express();

app.get('/', function(req, res){
    var obj = req.query;    //可以获取到传过来的数据
})
```



## 2. POST请求获取参数的方式

1. post请求，因为express中没有内置的获取数据的API，所以需要第三方包来借助完成

   ### 2.1 安装第三方包 body-parser

   ```shell
   npm install body-parser --save
   ```

   ### 2.2 配置 body-parser

   ```javascript
   var express = require('express');
   
   var bodyParser = require('body-parser');   //导入第三方包
   
   var app = express();
   
   //配置代码
   app.use(bodyParser.urlencoded({extended: false}));
   
   app.use(bodyParser.json());
   
   app.post('/', function(req, res){
       var obj = req.body;    //获取到数据
   })
   ```

   
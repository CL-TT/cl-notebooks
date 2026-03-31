# Express-art-template模板引擎

## 1. 安装包

```shell
npm install art-template --save
npm install express-art-template --save
```



## 2. 基本代码

```javascript
var express = require('express');

var app = express();

app.engine('art', require('express-art-template'));    //这是核心代码

app.get('/', function(req, res){
    res.render('login.html');   //渲染方法
})
```



## 3. 原理解释

```javascript 
//1. 这里不用加载art-template，但是还是要下载，因为express-art-template是依赖于art-template的

//2. app.engine('art')   第一个参数表示当渲染.art结尾的文件时，使用art-template模板引擎
//   这里写什么，views里面的文件就要以什么结尾

//3. Express为 response对象提供了render方法，但默认不可用，但是配置了模板引擎就可以使用render函数

//4. res.render('login.html');  这里直接写文件名，因为会默认到views目录下面查找文件

//5. 如果想修改默认的views目录，  那么可以设置
app.set('views', '你想修改的默认路径');
```


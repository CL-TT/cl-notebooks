# 中间件的使用

## 1.概念

1. 每一个请求路径在程序中都会映射一个方法， 这些方法被称为中间件
2. 匹配到了请求，会移交给第一个处理函数， 函数中需要手动交给后续中间件进行处理
3. 一个请求可以有多个中间件
4. 如果中间件中出错， 不进行处理的话， 那么后续程序就无法执行， 并响应500状态码
5. 如果进行了处理，那么后面就会正常进行



## 2.代码演示

```javascript
//创建错误处理中间件
module.exports = (err, req, res, next) => {
    if(err){
        const errObj = {
            status: 500,
            mes: err instanceof Error? err.message : err
        }
        
        res.status(500).send(errObj);
    }else{
        next();
    }
}
```



```javascript
const erpress = require('express');

const app = express();

app.listen('9999', () => {
    console.log('服务已经启动')
});

app.get('/new', (err, req, res, next) => {
    console.log(1);
    
    next();
    
    next(new Error('出错了'))
}, (err, req, res, next) => {
    console.log(2);
    
    next();
})


//使用中间件
app.use('/new', require(''));
```





## 3.常用中间件

```javascript
//1. express.static(path)   访问静态资源
const path = require('path');

const publicPath = path.resolve(__dirname, '../public');

//第一个参数是根本路径， 可以不写
//如果映射的是目录的话， 则自动找index.html这个文件
app.use('/static', express.static(publicPath));
```


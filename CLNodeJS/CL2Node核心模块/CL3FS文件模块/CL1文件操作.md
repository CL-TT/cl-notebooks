# 文件操作

## 1.fs模块

在node中想要进行文件操作， 就必须引入fs这个核心模块， 他提供了文件所有相关操作的API

var  fs = require('fs');           //引入fs模块



## 2.读文件

```javascript
var fs = require('fs');         //引入fs模块

fs.readFile('路径', function(error, data){
    if(error){  //如果读取失败，那么就打印错误对象
        console.log(error);
    }else{
        console.log(data);   //读取成功，那么就打印读取成功的数据
    }
})
```



## 3.写文件

```javascript
var fs = require('fs');

fs.writeFile('路径', '内容', function(error){
    if(error){
        console.log('写入失败');
    }else{
        console.log('写入成功');
    }
})
```



## 4.读取一个文件的目录

```javascript
fs.readdir('目录路径', function(error, file){
    if(error){
        console.log('目录不存在');
    }else{
        console.log(file);   //file是一个数组
    }
})
```


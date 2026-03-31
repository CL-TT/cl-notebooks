# 详解CMD

## 1. CMD的使用

```js
1. sea.js实现了 CMD的规范

2. script标签导入sea.js

3. 然后再script标签中写   seajs.use('主入口js文件')

4. 基本使用{
    1. define(function(require, exports, module){
          let a = require('')    //导入模块
          module.exports = {}   //导出数据
       })
    
    2. define(function(require, exports, module){
          require.async('', function(){})
          //也可以用异步的导入方式
       })
  }

5.把模块文件添加以script标签的模式，  但是添加之后立马删除script标签， 但是把导出的数据, 已经保存到了内存中
```



1. 因为为了规范去靠近commonjs的写法， CMD率先弄出了这种写法，  后来AMD也支持了这种写法
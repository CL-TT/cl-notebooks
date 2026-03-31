# CommonJs模块化原理

## 1.模块是如何的查找的

1.  可以通过绝对路径进行查找

   ```javascript
   require('绝对路径名称');
   ```

2.  相对路径是相对于当前模块来说， 一般是导入自己的模块，，   他同样会把相对路径转化为绝对路径

   ```javascript
   require('相对路径');
   //在这个函数内部，会把相对路径转化为绝对路径
   ```

3. 还有一种相对路径

   ```javascript
   require('abc');
   
   //他的查找顺序是这样的
   
   //1. 检查是否是内置模块， 例如 path  fs 等
   //2. 如果不是内置模块， 会检查当前目录下的node_modules又没有这个模块， 如果找不到就到上一层的node_modules目录下去找， 最终都找不到就会报错
   ```

4. 关于查找时有没有写后缀名

   ```javascript
   //如果没有写后缀名， 自动给您补全，  顺序是  .js,   .json,    .node,    .mjs
   ```

5. 关于查找时有没有写文件名

   ```javascript
   //如果只写了目录名，没有写文件名， 则自动寻找该目录下的index.js
   
   require('./src');
   //会先认为src是一个文件， 找不到时才会认为是一个目录
   // ./src.js  =>  ./src.json  => ./src.node => ./src.mjs =>  ./src/index.js
   ```



## 2.使用require函数时发生了什么

```javascript
require(modulePath);

//1. 第一步会把传入的路径转化为绝对路径，  通过require函数中的  resolve方法

//2. 第二步会查看缓存中又没有加载过这个模块 require.cache[路径], 如果缓存中有的话，就返回结果

//3. 第三步如果缓存中没有的话，会把这个模块的内容包裹到一个函数中  temp(module, exports, require)

//4. 创建module对象，   执行 module.exports = {},   exports = module.exports

//5. 调用这个temp函数  temp.call(module.exports);   让这个函数的this指向module.exports
```


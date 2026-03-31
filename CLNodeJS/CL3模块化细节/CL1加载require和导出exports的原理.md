# 加载require和导出exports的原理

## 1. 加载和加载规则

```javascript
//加载使用 require 来加载模块
//1. 核心模块   fs   http    url     os     path等
//2. 第三方模块  art-template等
//3. 自己定义的模块 
var obj = require('')
//这句代码有两层意思
//1. 加载模块里面的代码
//2. 返回模块中导出的对象，返回了一个对象


//2.加载规则
//2.1 自己定义的模块，优先在缓存中加载的
main.js
 require('a.js');
 require('b.js'); 
//这个main.js会打印  a ,  b    b只会打印一次， 因为会优先从缓存中加载，会看这个模块有没有被加载过
//被加载了之后就不会再加载了，只会得到他导出的对象。

a.js
 console.log('a');
 require('b.js');

b.js
 console.log('b');

//2.2 第三方导入的模块
//1.会先找到当前文件所处目录的node_modules目录
//2.然后再找到node-modules/art-template(第三方模块叫什么名字就找什么目录)
//3.然后再找到node_modules/art-template/package.json文件
//4.然后再找到node_modules/art-template/package.json/main属性
//5.main属性记录了art-template的入口模块
//6.如果没有main属性，就会一个默认的备选项 index.js
//7.如果这些都不成立，则会向上一级的node_modules中查找，重复上述步骤
//8.最终没有找到，就会报错
```

## 

## 2. 导出

```javascript
var name = 'caolei';   

//想要导出这个变量有两种方法

//1. exports.name = name;     
//exports可以导出多个变量或者方法

//2. module.exports = name;
//module.exports可以直接导出变量， 直接把变量名给他， 如果导出多个会产生覆盖
```



## 3. exports和module.exports的基本原理

```javascript
//1. 在模块中会有一段隐式的代码

this => module.exports

var module = {
    exports:{
        
    }
}

return module.exports;

//1.实际上在模块中返回的是 module.exports这个对象， 但因为node官方认为module.exports.name = name
//这样的方法过于繁琐， 所以又隐式的加上了
exports = module.exports      //因为这样是引用值，所以他两共用了同一个对象

//2.因为返回的是module.exports这个对象， 所以module.exports可以直接导出变量


//3.为什么exports不能直接导出变量
//因为如果执行 exports = name   那么exports就不指向 module.exports这个对象了，他们之间就不产生联系了
//而又因为返回的是module.exports这个对象， 所以就不会导出name这变量


```


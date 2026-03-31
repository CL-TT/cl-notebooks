# 详解AMD

## 1. AMD的使用

```js
1.导出  

1.1 直接导出数据 define({})， define('a')  
1.2 定义函数使用return导出 define(function(){return})


2. 第一种导入方式  define(['模块名称1', '模块名称2'], function(参数1=>指代的是模块返回的内容){})

3. 第二种导入方式 

define(function(require, exports, module){

     let obj = require('')   //导入模块

     、、、、、、自己代码

     module.exports = {}   //导出的数据

   })

}

4.AMD同样也有缓存机制
```



```html
// 1. 需要借助一个require.js文件

// 2. data-main 是入口文件

<script src="./require.js" data-main="./index.js"></script>
```

```js
index.js文件

define(['a'], function(a){
    console.log('a模块的数据', a)
})
```

```js
a.js文件

define(function(){
    let name = 'aa';
    
    return {
        name
    }
})
```


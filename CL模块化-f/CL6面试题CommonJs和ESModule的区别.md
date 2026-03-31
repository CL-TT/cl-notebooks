# CommonJs和ESModule的区别

## 1. CommonJs

1. 社区标准，并非官方标准
2. 使用函数实现
3. 仅node环境支持
4. 动态依赖（需要代码运行后才能确定依赖）
5. 动态依赖是同步的

```js
require函数的伪代码

function require(path) {
    if (该模块有缓存吗) {
        return 缓存结果
    }
    
    function _run(exports, require, module, __filename, __dirname) {
        // 模块代码放入这里
    }
    
    var module = {
        exports: {
            
        }
    }
    
    _run.call(module.exports, module.exports, require, module, 模块路径， 模快所在目录)
    
    return module.exports
}


exports 和 this 和 module.exports是一个东西


但是给module.exports = { }   这个就不一样了
```



## 2. ESModule

1. 官方标准
2. 使用新语法实现
3. 所有环境均支持
4. 同时支持静态依赖和动态依赖， 静态依赖在代码运行前就要确定依赖关系
5. 动态依赖是异步的



## 3. require和import的区别？  面试题

1. ##### 规范不同

   require是CommonJs/AMD规范

   import是ES6模块化规范

2. ##### 调用时间不同

   require是运行时调用，所以require可以运用在代码的任何地方

   import是编译时调用，所以必须放在文件开头

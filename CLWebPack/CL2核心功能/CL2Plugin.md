# Plugin的使用

## 1. plugin是什么，以及作用

1. **plugin本质上是一个带有apply方法的对象**， 这个对象一般用构造函数来实现，当然也可以用类来实现
2. loader的定位只是起到转换代码的作用，而一些其他的操作难以用loader来完成， 比如说当webpack生成文件时，我要做一些额外的事情，就是说需要把功能嵌入到webpack的编译流程中，这种功能的实现是依托于plugin的



## 2. plugin的使用以及配置

```js
// 自定义plugin

module.exports = class MyPlugin {
    apply(compiler){
        
    }
}
```

```js
// 配置
const MyPlugin = require('');

module.exports = {
    plugins: [
        new MyPlugin()
    ]
}
```



## 3. compiler和compilation

1. apply函数会在初始化阶段，创建好Compiler对象之后运行
2. compiler对象是在初始化阶段构建的，整个webpack打包期间只有一个compiler对象，后续完成打包工作的是compiler对象内部创建的compilation
3. compiler对象提供了大量的钩子函数（hooks, 可以理解为事件）plugin的开发者可以注册这些钩子函数，参与webpack的编译和生成
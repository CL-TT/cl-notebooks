# 区分环境

有时候需要为开发环境和生产环境分别写配置



## 1. 解决方案

```js
#1. 写两个配置文件， 然后在package.json文件中如下配置

1. "dev": "webpack --config webpack.dev.js"
2. "build": "webpack --config webpack.build.js"

#2. 配置导出的不仅仅是一个对象，还可以是一个函数

env的值是在命令行里面配置的  =>  npx webpack --env.pro  

module.exports = function (env) {
    //可以通过判断env的值来决定返回什么样的配置对象
    if(env && env.pro){
        return {}
    }else{
        return {}
    }
}
```


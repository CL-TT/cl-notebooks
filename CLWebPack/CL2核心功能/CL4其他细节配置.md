# 其他细节配置

```js
#1. context

module.exports = {
    context: path.resolve(__dirname, 'src') //这样配置之后，就会基于src路径
}

该配置会影响入口和loaders的解析，入口和loaders的相对路径会以context的配置作为基准路径，这样你的配置，会独立于CWD(当前执行路径)


#2. library 设置这个配置，打包后的结果中，会将自执行函数的执行结果暴露给abc

module.exports = {
    library: 'abc'
}


#3. libraryTarget 该配置可以更加精细的控制如何暴露入口包的导出结果

module.exports = {
    libraryTarget: 'var'
}

// var: 默认值，暴露给一个普通的变量
// window: 暴露给window对象的一个属性
// this: 暴露给this的一个属性
// global: 暴露给global的一个属性
// commonJs: 暴露给exports的一个属性



#4. target 设置打包结果最终要运行的环境

常用值有web, node

module.exports = {
    target: 'web' // 默认值
}



#5. noParse 不解析正则表达式匹配的模块，通常用它来忽略那些大型的单模块库，以提高构建性能

// 那个模块的代码，以及依赖都不解析， 就是直接从读取文件到把代码保存到模块记录表中，其他的过程全部不要

module.exports = {
    noParse: //
}
    
    
    
#6. resolve

module.exports = {
    modules: [],   //查找模块的路径
    extensions: ['.js', '.json'],  //文件后缀，可以不用写文件后缀，webpack会帮你自动补全
    //引入文件时起的别名
    alias: {
        "@": path.resolve(__dirname, 'src');
    }
}
    
    
#7. externals
这比较适用于一些第三方库来自于外部CDN的情况，这样一来，既可以在页面中使用CDN， 又可以让打包的体积变的更小，还不影响源码的编写
    
module.exports = {
    jquery: '$',
    loadsh: "_"
}

这样jquery和loadsh的代码就不会进行打包，可以减少打包体积
```


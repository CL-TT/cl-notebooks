# Loader的使用

## 1. loader是什么，以及作用

1. loader本质上就是一个函数，作用是把传入的源代码字符串（字符串形式）通过一系列处理，返回处理后的代码字符串
2. loader处理，是在编译过程中的读取文件内容之后， 进行抽象语法树分析之前



## 2. 处理loader的流程

1. 当前模块是否满足某个规则（一般是正则表达式）
2. 如果满足，读取规则中对应的loaders，形成loaders数组，如果不满足就是空数组
3. 先把源代码交给数组的最后一个loader，再依次向前递交



## 3. loader的使用

1. 不能在loader中使用es模块化标准， 因为loader是在打包的过程中运行的，打包的过程是在node环境中
2. loaders数组，一定是从后往前

```js
module.exports = (sourceCode) => {
    return sourceCode
}
```

```js
// 配置文件 webpack.config.js

module.exports = {
    module: {
        rules: [
            // 规则1
            {
                test: /\.js$/,  // 匹配所有js文件
                use: ['./loaders/loader1', './loaders/loader2']
            },
            // 规则2
            {
                test: /index\.js$/,   // 匹配index.js文件
                use: [
                    {
                        loader: '',   //模块路径
                        options: {
                            //向loader传递的额外参数
                            //可以通过安装loader-utils来解析这个options
                            //let options = loaderUtils.getOptions(this)
                        }
                    }
                ]
            }
        ]
    }
}
```


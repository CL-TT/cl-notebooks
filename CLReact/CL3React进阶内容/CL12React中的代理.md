# React中的代理

## 1. 步骤

1. 在src目录下新建setupProxy.js文件
2. npm i http-proxy-middleware 库



```react
const { createProxyMiddleware } = require('http-proxy-middleware')

module.exports = function (app) {
    app.use(createProxyMiddleware('/res', {
        target: '指向具体的地址',
        changeOrigin: true
    }))
}
```



## 2. 注意事项

1. 要注意http-proxy-middleware是什么版本，3.x版本会出现页面打不开的现象
2. 用的是2.x的版本就可以
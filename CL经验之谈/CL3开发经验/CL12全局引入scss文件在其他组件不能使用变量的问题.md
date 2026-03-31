# 全局引入scss文件但在其他组件中不能使用scss变量的问题

## 1. 老版本的vue项目解决方案

```
1. 安装：sass-resources-loader （如果不安装的话变量会报错）

npm install sass-resources-loader --save-dev

2. 在build/utils.js文件中

scss: generateLoaders('sass').concat(
    {
        loader: 'sass-resources-loader',
        options: {
          resources: path.resolve(__dirname, '../src/style/share.scss') //这里写自己的文件路径
        }
     }
)
```



## 2. 新版本的vue项目解决方案

```
 配置vue.config.js文件，使得全局都能使用scss变量，而不用每个文件单独引入
 
 
css: {
    loaderOptions: {
      // 默认情况下 `sass` 选项会同时对 `sass` 和 `scss` 语法同时生效
      // 因为 `scss` 语法在内部也是由 sass-loader 处理的
      // 但是在配置 `prependData` 选项的时候
      // `scss` 语法会要求语句结尾必须有分号，`sass` 则要求必须没有分号
      // 在这种情况下，我们可以使用 `scss` 选项，对 `scss` 语法进行单独配置
      scss: {
        prependData: `@import "~@/stylesheet/variable.scss";`
      }
  }
}
```


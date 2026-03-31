# babel详解

## 1. 为什么会有babel,babel出现的背景

babel又被称为巴别塔，巴别塔象征着统一的国度统一的语言，而今天的js世界缺少一座巴别塔，不同版本的浏览器能识别的ES标准并不相同，就导致了开发者在面对不同版本的浏览器要使用不同的语言，这就是前端开发面临的困境。

babel的出现就是解决这个问题，**它是一个编译器**，可以把不同标准书写的语言编译为统一的，能被各种浏览器识别的语言。



babel它本身仅提供一些分析功能，真正的转换需要依托于插件完成



## 2. babel的安装和使用

#### 安装

babel可以和webpack一起使用，也可以独立使用，如果需要独立使用的话，需要安装下面两个库

@babel/core:   babel的核心库，提供了编译所需的所有api

@babel/cli:  提供一个命令行工具，调用核心库的api完成编译



npm i @babel/core @babel/cli



#### 使用

@babel/cli提供了一个的命令 babel

按文件编译：babel 要编译的文件 -o 编译结果文件

按目录编译：babel 要编译的整个目录 -d 编译结果放置的目录



## 3. babel的配置

babel本身没有做任何事情，真正的编译要依托于babel插件和babel预设来完成，是通过一个配置文件.babelrc

```js
{
    "presets": [
        "@babel/preset-env"
    ],
    "plugins": []
}

除了预设可以转换代码，插件也可以转换代码，顺序是

插件在预设之前运行， 插件运行的顺序是从前往后，预设则相反
```



## 4. 在webpack中使用

```js
module.exports = {
    module: {
        rules: [
            { test: /\.js$/, use: "babel-loader" }
        ]
    }
}
```


# 对NPM的理解

## 1. 什么是NPM

1. npm全称为node package manage, 即node包管理器， 它运行在node环境中。



## 2. NPM为什么要运行在node环境中？面试题

```js
1. npm是运行在node环境中的， 不是运行在浏览器环境中的， 因为浏览器环境无法提供对文件的下载，删除，读取等功能， 而node是属于服务器环境，没有浏览器环境的种种限制，理论上可以完全掌控运行node的计算机。
```



## 3. NPM的组成

1. registry：入口

​    可以把它想象成一个庞大的数据库

​    第三方库的开发者，将自己的库按照 npm 的规范，打包上传到数据库中

​    使用者通过统一的地址下载第三方包

​    2. 官网：https://www.npmjs.com/

​    查询包

​    注册、登录、管理个人信息

​    3. CLI：command-line interface 命令行接口

​    安装好 npm 后，通过 CLI 来使用 npm 的各种功能,  写各种命令来使用npm
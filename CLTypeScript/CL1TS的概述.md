# TS的概述

## 1. 为什么要学TS?

1. 能获得更好的开发体验
2. 解决js中难以解决的问题



## 2. JS开发中存在的问题

1. 在开发的过程中，**使用了不存在的变量或者函数**，在开发时如果不注意的话是发现不了这种问题的，这种错误只有在运行的过程中才知道出错了
2. 类型错误，**一个不确定的类型当成了一个确定的类型处理**， js是弱类型语言
3. undefined和null

```js
const a = undefined;

a.name 就会报a prototype name is not defined
```



## 3. TS语言的特点

1. **TS是JS语言的超集，是一个可选的，静态的类型系统**
2. 类型系统（对代码中的所有标识符进行类型检查，例如变量，函数，参数，返回值等）
3. 可选的， 我不一定要使用你TS
4. 不管是node环境还是浏览器环境，都不能识别TS语言，tsc => es， （都需要借助第三方库的支持才能识别）
5. **静态的类型检查（是在tsc => es这个编译的过程发现错误， ts是不参与任何运行时的类型检查的）**



## 4. 自行搭建TS环境流程

1. 后面不管是vue还是react，应该都集成了ts环境，不需要自己搭建ts环境

```js
1. npm i -g typescript
2. tsc --init > 弄出配置文件
3. 自定义配置项
4. 安装 npm i -D @types/node
5. npm i -g ts-node
6. npm i -g nodemon


ts-node: 这个库起的作用是，将ts代码在内存中完成编译， 同时完成运行, ts-node src/index
nodemon检测文件的改变  nodemon --watch src -e ts --exec ts-node src/index.ts
-e ts 只监控文件后缀名为ts的， --watch src 只监控src文件夹下面的文件
```



## 5. TS的几种假设

1. 会假设当前执行的环境是Dom环境
2. 如果代码中没有使用模块化语句（import， export），便认为该代码是全局执行
3. 编译的目标代码是ES3
# TS中的模块化

## 1. 在TS中如何使用模块化

在TS中，可以使用ES6模块化

```ts
导出部分

export const name:string = 'caolei'

export function add() {
    
}


导入

import { name, add } from ''
```





## 2. TS编译结果中的模块化

在TS中模块的导入导出尽量都使用ES6模块化

这个可以在tsconfig.json文件里面配置， module属性

```json
{
  //编译的选项, 
  "compilerOptions": {
    "target": "es2016",    //编译的目标代码是es7    默认的是es3
      
    "module": "commonjs",  //编译结果中的模块化
      
    "lib": ["es2016"],     //运行环境， 虽然去掉了浏览器环境，但他也不知道这是node环境，需要安装@types/node
    "outDir": "./dist",    //可以选择编译输出的结果在哪个具体的目录
    "removeComments": true, //编译结果移除注释
    "noEmitOnError": true,  //错误是不生成编译结果
    "esModuleInterop": true, //启动es模块化交互非es模块导出
    "moduleResolution": 'node'  //设置解析模块的模式 node,  1.经典模式 2 node模式
  },

  "include": ["./src"]   // 编译时会编译所有文件，可以指定文件夹, 选择哪些文件夹下的ts文件可以被编译
}


1. 如果配置的是ES6模块化，那么和原代码没有什么区别

2. 如果配置的是CommonJs模块化，那么导出的变量都会变成exports下面的属性，默认导出会变成exports下面的default属性
```



## 3. 解决默认导入的错误

```ts
在TS中这样写是会报错，就是因为fs模块使用CommonJs的导出语句, 它是默认导出

import fs from 'fs'

在编译结果中

const fs_1 = require('fs')

fs_1.default.readFileAsync()

fs_1里面根本就没有default这个属性


解决办法

1. 使用具名导入

import {} from 'fs'

2. 修改配置文件，启动es模块化交互非es模块导出， 修改后就可以正常使用
```



## 4. 在TS中使用CommonJs代码

```ts
导出

export = {
    name: ''
}


导入

import xx = require('')
```



## 5. 模块解析规则

1. 经典型

2. node

   2.1 相对路径 require('./xx')

   先在当前文件夹下面找xx文件， 没有找到， 在package.json中找main字段，在xx文件夹下面找main字段对应的文件名， 还没有就在xx文件夹下面找index.js

   

   2.2 非相对路径 require('xx')

   先在当前文件夹下面找node_modules,看这下面有没有xx文件， 如果没有，就找xx文件夹，看文件夹下面的package.json中的main字段，如果没有就找xx文件夹下面的index.js， 如果没有node_modules就向上找node_modules，其他的规则一致

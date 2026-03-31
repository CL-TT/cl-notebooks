# 安装项目使用npm install 遇到的问题

## 1. node-sass

```js
Module build failed: Error: ENOENT: no such file or directory, scandir 'D:\Commu nity\D4Mobile\node_modules\node-sass\vendor'

解决办法

1.cd 进入node_modules
$ cd node_modules

2.运行npm rebuild node-sass
$ npm rebuild node-sass
```



## 2. chromedriver

```
npm ERR! code ELIFECYCLE npm ERR! errno 1 npm ERR! chromedriver@2.46.0 install: `node install.js`

解决办法

1. npm install chromedriver@2.46.0 --ignore-scripts

2. npm install
```


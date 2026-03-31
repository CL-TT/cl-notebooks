# WebPack的安装和使用

## 1. WebPack的安装

1. 提供两个包

   webpack

   webpack-cli

   npm i  webpack webpack-cli -D     用于开发环境

   yarn add webpack webpack-cli -D



## 2. WebPack的基本使用

1. npx  webpack    =>   默认是生产环境

   默认情况下webpack会以  ./src/index.js作为入口文件，  会打包到  ./dist/main.js

   

2. 可以通过 --mode=''  来配置webpack的运行环境是什么

   ```javascript
   //1. 在package.json文件中
   
   "scripts": {
       "dev": "webpack --mode=development",
       "build": "webpack --mode=production"
   }
   ```

   

### 3. 打包运行环境是在node环境下
# 其他的包管理器

## 1. cnpm的理解

#### 1. 为什么会出现cnpm?

**为了解决国内用户连接npm registry缓慢的问题**， 淘宝搭建了自己的registry，即淘宝镜像源， 过去npm没有提供修改registry的功能， 所以淘宝提供了一个cli工具即cnpm, 它除了不支持发布包的命令，其他命令都支持，  只不过连接的是淘宝的镜像源



现在官方提供了修改registry的功能， 所以cnpm的作用也就没有那么大了

```js
npm install cnpm -g
```





## 2. pnpm的理解

#### 1. pnpm的概述

1. pnpm是一种新起的包管理器， 它具有以下几种优势
2. **安装效率要高于npm和yarn**
3. **及其简洁的node_modules目录**（你只依赖了A，那么目录下就只有A， 他的间接依赖不会再出现在目录下）
4. **避免了开发时使用间接依赖**(比如你依赖了A包， A包依赖了B包， 你又想使用B包， 在npm和yarn中， 直接使用B包是不会报错的， 但是在pnpm中是会报错的)
5. **能极大的降低磁盘空间的占用**
6. npm install pnpm -g,     命令和npm基本一样



#### 2. pnpm的原理

1. 同yarn和npm一样， pnpm仍然使用缓存来保存已经安装过的包，以及使用pnpm-lock.yaml来详细记录依赖版本
2. 不同于yarn和npm, pnpm使用符号链接和硬链接（可以想象成一种快捷方式）的做法来放置依赖，从而规避了从缓存中拷贝文件的时间，使得安装和卸载的速度更快
3. 由于使用了符号链接和硬链接，pnpm可以规避windows操作系统路径过长的问题，因此，他选择使用树形的依赖结果，有着几乎完美的依赖管理， 正因如此，只能使用直接依赖，不能使用间接依赖

#### 3. 注意事项（不是因为我不完美， 而是这个世界配不上我的完美）

1. 以前的包可能会有那种间接依赖的写法， 所以使用pnpm安装的话，就会出现问题





## 3. nvm的理解

#### 1. 什么是nvm?

1. nvm是一个用于切换node版本的工具
2. 下载 https://github.com/coreybutler/nvm-windows/releases
3. 下载nvm-setup.zip后直接安装

#### 2. nvm的使用

1. 为了下载速度，全部改成node淘宝镜像https://npm.taobao.org/mirrors/node/， npm淘宝镜像https://npm.taobao.org/mirrors/npm/  使用命令 nvm node_mirror  url,    nvm  npm_mirror  url

 https://registry.npmmirror.com 新的淘宝镜像

1. 切换版本  nvm use 版本号
2. 卸载某个版本  nvm  uninstall 版本号
3. 下载某个版本  nvm install  版本号
4. 查看所有的node版本  nvm list   
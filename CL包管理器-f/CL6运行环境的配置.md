# 运行环境的配置

## 1. 配置环境的几种方式

### 临时配置

1. 代码可能在不同的环境下运行，如何在代码中得知此时是什么环境

```js
1. set是window环境下的命令
2. export 是苹果环境下的命令

script: {
    "start": "set NODE_ENV==development"
}

3. 有一个工具是可以跨环境的  cross-env NODE_ENV=test
```


# 创建React项目并使用

这两种方式都不集成路由和redux

## 1. Vite创建React项目

这种方式可能需要node 20版本以上

```js
# 使用 npm
npm create vite@latest my-react-app -- --template react

# 若用 TypeScript（推荐，有类型提示）
npm create vite@latest my-react-app -- --template react-ts

# 若用 yarn
yarn create vite my-react-app --template react
```



## 2. 使用create react-app创建

```js
# npm
npx create-react-app my-react-app

# TypeScript 版本
npx create-react-app my-react-app --template typescript

# yarn
yarn create react-app my-react-app
```



## 3. 在react项目中使用less或者sass

```js
1. 安装依赖  yarn add less   yarn add sass

2. 直接使用 less和sass一样，都是直接使用

index.less

div{
    p{
        color: ''
    }
}

index.jsx中直接引入index.less即可
```


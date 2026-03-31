# tailwindcss的使用

## 1. 什么是tailwindcss

tailwindcss是一个实用工具优先的 CSS 框架，它通过组合大量低级实用类来构建高级组件和页面结构，而不是通过传统的CSS选择器直接写入样式规则。这种方法使得开发者能够快速构建美观且高度可定制的网站界面‌



## 2. Tailwind CSS在vue项目中的使用

```js
1. 安装库

npm i tailwindcss postcss autoprefixer

yarn add tailwindcss postcss autoprefixer


2. 生成配置文件，以及一些必要的文件

#1. npx tailwindcss init  并进行配置

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./index.html",
    "./src/**/*.{vue,js,ts,jsx,tsx}"
  ],
  theme: {
    extend: {},
  },
  plugins: [],
  corePlugins: {
    preflight: false
  }
}

#2. 在根目录下创建一个postcss.config.js

module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}

#3. 在src目录下创建tailwind.css文件

@tailwind base;
@tailwind components;
@tailwind utilities;

#4. 在main.js中引入tailwind.css

#5. 在配置文件中添加如下配置
module.exports = {
  css: {
    loaderOptions: {
      postcss: {
        plugins: [
          require('tailwindcss'),

          require('autoprefixer')
        ]
      }
    }
  }
}

3. 然后在项目中进行验证

<div class="font-bold"></div>
```



## 3. 使用过程中遇到的问题

####   1. 添加类名样式不生效的问题  （版本问题）

```js
Module build failed (from ./node_modules/postcss-loader/src/index.js):
Error: PostCSS plugin tailwindcss requires PostCSS 8.
Migration guide for end-users:
https://github.com/postcss/postcss/wiki/PostCSS-8-for-end-users


解决办法：
npm uninstall tailwindcss postcss autoprefixer
npm install tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
```



####   2. 引入tailwindcss之后，h1,h2 标题样式失效问题

```js
原因是tailwindcss对这些样式做了预处理

h1,
h2,
h3,
h4,
h5,
h6 {
  font-size: inherit;
  font-weight: inherit;
}


解决办法：

1. 去掉预处理

在tailwind.config.js中配置

module.exports = {
  corePlugins: {
    preflight: false,
  }
}
```


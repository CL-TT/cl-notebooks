# 绑定元素的方式以及插值表达式

## 1.绑定元素的方式

1. el : '#app'
2. app.$mount('#app')   可能会在一些延迟操作中发挥作用

```javascript
const app = new Vue({
    el: '#app'
})

app.$mount('#app')
```



## 2.插值表达式

```javascript
{{ 数字 }}
{{ 字符串 }}
{{ boolean }}
{{ 数组 }}
{{ 对象 }}
{{ undefined }}
{{ 表达式}}
```


# v-if和v-show的区别

## 1.v-if

```javascript
1. v-if可以配合着v-else  v-else-if使用，并且可以在template上使用

2. v-if更适用于初次渲染， 因为当条件不成立时，他是不会渲染元素的， 他对元素显示与隐藏的切换有着更大的开销
```





## 2.v-show

```javascript
1. v-show不可以配合v-else   v-else-if   并且不可以在template上使用

2. v-show更适用于频繁切换显示与隐藏的状态，  因为当条件不成立时，他会给元素设置css属性display: none来隐藏元素，  所以他有更大初始渲染的开销
```


# 获取Dom元素的方法

## 1.实时的

```javascript
//1. document.getElementById()    根据id来获取元素

//2. document.getElementsByClassName()    根据class来获取元素
//   返回值是一个类数组

//3. document.getElementsByTagName()    根据标签名来获取元素
//   返回值是一个类数组

//4. document.getElementsByName()    根据属性名来获取元素
//   返回值是一个类数组
```



## 2.非实时的

```javascript
//1. document.querySelector('div p')   //可以根据css选择元素的方式来获取元素
//   这个只会获取到第一匹配的，并且这个方法不是实时的

//2. document.querySelectorAll()   //可以获取到到各元素
//   返回值是一个类数组，方法也不是实时的
```


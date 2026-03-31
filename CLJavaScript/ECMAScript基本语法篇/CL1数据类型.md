# 数据类型

## 1. 原始值

```javascript
1. Number  数字类型
var number = 3;
typeof(number) => Number

2. String   字符串类型
var str = '曹磊';
typeof(str) => String

3. Boolean  布尔类型
var target = true;
typeof(target) => Boolean

4. undefined   未定义类型
var str = undefined;
typeof(str) => undefined;
//未定义或者未赋值
var a;
typeof a  =>  undefined      //未赋值
typeof b =>  undefined       //未定义

5. Null    空类型  值为空
var str = null;
typeof str  =>  Object   //这个是历史遗留问题

```

## 2. 引用值

```javascript
1. 对象
1.1 Object  
var obj = {
    name: 'caolei',
    age: 22
}
typeof obj => Object

1.2 Array   数组也是对象的一种
var arr = [];
typeof arr => Object


2. function  函数   其实函数也是一种对象
var a = function(){}
typeof a => function
```


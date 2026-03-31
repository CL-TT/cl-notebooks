# 包装类

## 1.包装类简介

1. JS为了增强原始类型的功能，为boolean、string、number分别创建了一个构造函数：

1. Boolean
2. String
3. Number

如果语法上，将原始类型当作对象使用时（一般是在使用属性时），JS会自动在该位置利用对应的构造函数，创建对象来访问原始类型的属性。



```javascript
let number = 3.243890

number.toFixed(2);   //把数字保留两位小数会打印 3.24

//number类型本应该是没有属性的，但在这里有了包装类这一说法，就创建了如下
var num = new Number(number);
num.toFixed(2);
```


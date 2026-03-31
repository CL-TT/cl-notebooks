# instanceof详解

## 1. instanceof的作用

instanceof只能运用与对象，而不能运用于原始值，运用于原始值都是会报false的

```javascript
var str = 'string';
str instanceof String;     //false

var num = 2;
num instanceof Number;    //false
```

A instanceof B

判断A对象是否是B构造方法构造出来的

A的原型链上有没有B的原型，B的父类也可以



## 2.instanceof的演示

### 2.1基本构造函数的演示

```javascript
function A(){};
function B(){};
var a = new A();
var b = new B();

console.log(a instanceof A);    //true
console.log(b instanceof B);    //true
console.log(a instanceof B);    //false
```



### 2.2基本原理图

<img src="D:\CL武林秘籍\画图\instanceof.png"  />


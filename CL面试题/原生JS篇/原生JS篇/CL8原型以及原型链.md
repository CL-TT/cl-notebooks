# 原型以及原型链

1. **所有的对象都是通过new 函数创建的**
2. 函数又是一个对象，函数又是怎么来的呢， 函数是通过new Function创建的， （Function函数是在内存就创建好的）
3. 这就会导致（一个先有鸡还是先有蛋的问题）是先有对象还是先有函数呢， 
4. **所以Function函数是在内存一开始就创建的， 所以所有的函数都是通过new Function创建的， 所有的对象都是通过new  函数（Object, Array, String, Number）创建，  就是这么一个关系**



## 1. 原型(prototype)

1. **所有的函数对象都有一个属性**，prototype， 称之为函数原型，默认情况下，prototype就是一个普通的对象
2. 默认情况下，prototype有一个属性，constructor,他是一个对象，他指向构造函数本身



## 2.隐式原型(__proto__)

1. 所有的对象都有一个属性：__proto__, 称之为隐式原型
2. 默认情况下，隐式原型指向创建该对象的构造函数的原型

```js
1. 当你访问一个对象的成员时

#1. 看该对象自身是否拥有该成员， 如果有直接使用
#2. 看该对象的隐式原型是否有该成员，如果有直接使用

2. 猴子补丁：在函数原型中加入成员，以增强对象的功能，猴子补丁会导致原型污染，需谨慎使用。
```

![原型](D:\CL武林秘籍\画图\原型.png)



## 3. 原型链

1. 对象有一个属性（隐式原型）proto, 而这个proto又是一个对象，他也有自己的隐式原型，以此形成了原型链
2. Function的隐式原型指向自身的prototype
3. Object的原型的proto指向null     

![原型链](D:\CL武林秘籍\画图\原型链.png)



## 4. 原型链的应用

```js
1. getPrototypeOf 获取一个对象的隐式原型

let obj = {
    name: 'caolei'
}
console.log(Object.getPrototypeOf(obj) === Object.prototype);  // true


2. 对象 instanceof 函数  判断函数的原型是否在对象的原型链上 

3. Object.create()  创建一个新对象，其隐式原型指向指定的对象

4. Object.hasOwnProperty(属性名)  判断一个对象自身是否拥有某个属性
```

```js
应用

1. 类数组转换成真数组

Array.prototype.slice.call(类数组)

2. 实现继承


圣杯模式

let inherit = (function(){
   let Temp = function(){}
   
   return function(father, son){
       Temp.prototype = father.prototype;
       son.prototype = new Temp();
       son.prototype.constructor = son;
       son.prototype.super = father;
   }
})()

```


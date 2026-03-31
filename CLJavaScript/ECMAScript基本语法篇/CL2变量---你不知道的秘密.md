# 变量---你不知道的秘密

## 1. 变量声明提升

```JavaScript
var a = 3;   
//会拆分为
var a;
a = 3

console.log(b);   //不会报错，会打印undefined， 因为变量声明提升
var b = 4;

//会变成
var b;
console.log(b);
b = 4;
```

## 2. 全局变量都会变成window上的属性

```javascript 
//1. 如果变量名和已有的window属性名重名， 但是你没有赋值，只是声明，不会覆盖window上的属性
var console;      //只是声明没有赋值
console.log(console)   //打印的还是window上的console

//2. 如果赋值，undefined和null都行
var console = 'a';
console.log(console);    //会打印出字符串 'a'

//3. window有一个属性叫 name    值为空字符串
//就算赋值之后，也是转为字符串类型
var name;
console.log(name, typeof name);   //‘’   string


```


# 新增的数据类型symbol

## 1.符号Symbol的特点

1. Symbol是符号的意思，它是一种新增的数据类型

   每创建一个符号，都是一个新的符号，通过Symbol()函数生成的符号是永不相等的

```js
const sy = Symbol();

typeof(sy)  =>   "symbol"
```

2. 符号的设计初衷就是为了给对象设计私有属性，不想让别人访问的属性
3. 创建符号

```js
const sy = Symbol(描述性语言);

const obj = {
    name: 'caolei',
    age: 20,
    [sy]: 'hello world'
}
```

4. 符号是不可以被枚举的

```js
const sy = Symbol();

const obj = {
    name: 'caolei',
    age: 20,
    [sy]: 'hello'
}

for(let prop in obj){
    console.log(prop);
    //这只会输出name, age
}

Object.keys(obj)  同样也得不到符号属性
Object.getOwnPropertyNames   尽管这个方法可以得到无法枚举的属性，但是还是访问不了

ES6新增了一个方法可以读取符号
Object.getOwnPropertySymbols(obj)  他的返回值是一个数组
```



5. Symbol不能隐式转换，但是可以显示转换

```js
let sy = Symbol();

let str = String(sy);
```



## 2.共享符号Symbol.for()

```js
//1. 共享符号的创建, 共享符号只要描述信息是一样的，那么就是相同的
const sy = Symbol.for(描述信息);

const s1 = Symbol.for('caolei');
const s2 = Symbol.for('caolei');

console.log(s1 == s2);   //结果为true


//2. 模仿Symbol.for()的实现

const symbolFor = (() => {
    const global = {};
    
    return function(name){
        if(!global[name]){
            global[name] = Symbol(name);
        }
        
        return global[name];
    }
})
```



## 3.知名符号

1. 知名符号是一些具有特殊含义的共享符号
2. ES6延续了ES5的思想，减少魔法，多暴露内部的实现，所以知名符号的作用就是暴露某些场景的内部实现

```js
//1. Symbol.hasInstance  该符号的作用会影响到instanceof的判定
当运行 a instanceof A  就相当于  A[Symbol.hasInstance](a)

function A(){
    
}

const a = new A();
console.log(a instanceof A);   //运行结果肯定为true

//但是我就是想人为的改变其结果，在以前是没有办法办到的，但现在暴露了内部的实现是可以办到的

Object.defineProperty(A, Symbol.hasInstance, {
    value: function(){
        return false;
    }
})

console.log(a instanceof A);   //运行结果就是false


//2. Symbol.isConcatSpreadable   该知名符号会影响，数组的concat方法

const arr = [1, 2];
const arr2 = [8, 9];

const temp = arr.concat(arr2);  //结果就是 [1, 2, 8, 9]

但如果我设置了 arr[Symbol.isConcatSpreadable] = false;  就不会把数组进行展开

const temp = arr.concat(arr2);   //结果就是 [1, 2, [8, 9]]


//3. Symbol.toPrimitive  该知名符号会影响类型转换的结果, 默认情况下会先使用valueOf在使用toString
class A{
    constructor(num){
        this.num = num;
    }
}

const a = new A(34);

console.log(a + 'pp');   //[object Object]pp
console.log(a / 2);      //NaN
console.log(String(a));  //[object Object]

class A{
    constructor(num){
        this.num = num;
    }
    
    [Symbol.toPrimitive](type){
        if(type == 'default'){
            return this.num + '数字';
        }else if(type == 'number'){
            return this.num;
        }else if(type == 'string'){
            return this.num + '';
        }
    }
}

console.log(a + 'pp');   // '34数字pp'
console.log(a / 2);      // 17
console.log(String(a));  // '34'


//4. Symbol.toStringTag   该知名符号会影响Object.prototype.toString的返回值

class A{}

const a = new A();
console.log(Object.prototype.toString.call(a)); //会打印[object Object],但是我希望他输出 [object A], 就可以使用知名符号

class A{
    [Symbol.toStringTag] = 'A';
}

const a = new A();
console.log(Object.prototype.toString.call(a));  //[object A]
```


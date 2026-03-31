# Object的基本使用

## 1.Object的静态成员(构造函数里面的方法)

1. Object.keys(某个对象)，得到某个对象的所有属性名数组,   返回一个数组

```javascript
let obj = {
    name: 'caolei',
    age: 20,
    sex: '男'
}

let arr = Object.keys(obj);   // ['name', 'age', 'sex']
```



2. Object.values(某个对象)，得到某个对象的所有属性值数组， 返回一个数组

```javascript
let arr2 = Object.values(obj);    //['caolei', 20, '男']
```



3. Object.entries(某个对象)，得到某个对象的所有属性名和属性值的数组

```javascript
let arr3 = Object.entries(obj);   //[['name', 'caolei'], ['age', 20], ['sex', '男']]
```



    4. Object.is(data1, data2) 判断两个数据是否相等

```javascript
//Object.is  是按照严格模式进行比较的，  但是+0和-0是false
Object.is(2, '2');  //false   

Object.is(+0, -0) //false

+0 === -0  //true
```



## 2.实例成员

1. 实例成员可以被重写

**所有对象，都拥有Object的所有实例成员**

1. toString方法：得到某个对象的字符串格式 默认情况下，该方法返回"[object Object]";

```javascript
let obj = {};

obj.toString();     // '[object Object]'

let obj2 = new Object(2);
obj2.toString()   //如果是原始值的话，会先转为包装类，再使用toString()方法
```



2. valueOf方法：得到某个对象的值

```javascript
let obj = {};

obj.valueOf();   //返回的就是自己本身
```

默认情况下，返回该对象本身

> 在JS中，当自动的进行类型转换时，如果要对一个对象进行转换，实际上是先调用对象的valueOf方法，然后调用返回结果的toString方法，将得到的结果进行进一步转换。

obj.valueOf()已经变成123原始类型， 所以就不会调用toString()方法了


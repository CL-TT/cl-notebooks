# 了解Set，Map，WeakSet，WeakMap

## 1. Set

### 1.1 Set的基本概念

1. Set集合是专门用来保存多个数据的，但是不保存重复的数据，Set集合本身就是一个可迭代的对象

### 1.2 Set的基本使用

```js
//创建一个没有任何内容的集合
const s1 = new Set();

//创建一个具有初始内容的set集合，内容来自于可迭代的对象每一次迭代的结果
const s2 = new Set(iterable);

//s3的结果 1, 2, 3, 4,   不会有重复的结果
const s3 = new Set([1, 2, 2, 3, 4]); 

s1.add(1);    //添加一个数据到set集合末尾，如果数据已经存在则不作任何操作

s1.has();     //判断set集合中是否有此值

s1.delete();  //删除匹配的数据

s1.clear();   //清空整个set集合

s1.size();    //获取集合的长度，只读属性，无法重新赋值


1. 遍历set集合使用他自身的方法forEach

/**
* val: 是set集合中的每一个值
* index: 是索引
* set: set集合本身
**/
s1.forEach((val, index, set) => {
    console.log(val, index, set)
})

2. 使用for-of循环，因为Set集合本身就是可迭代的对象
for(let item of s1){
    console.log(item);
}
```

### 1.3 Set的应用

```js
//数组去重，字符串去重，我相信大家都不会陌生，以前我是借助对象的key值唯一来完成去重的，现在有了set不一样了

//数组去重
const arr = [1, 2, 2, 3, 3, 4];

const set = new Set(arr);

const newArr = [...set];   //输出的结果为  [ 1, 2, 3, 4 ]

//字符串去重
const str = 'aaabbccdd';

const set = new Set(str);

const newStr = [...set].join('');   //输出的结果为  'abcd'
```



## 2. Map

### 2.1 Map的基本概念

1. map集合专门用于存储多个键值对数据，数据集合的特点就是，键名一定不会重复

2. 以前用对象来保存键值对数据的缺点

   2.1 键名只能是字符串格式

   2.2 获取数据的数量不方便

   2.3 键名容易跟原型上的起冲突

### 2.2 Map的基本使用

```js
const map = new Map()

const map = new Map([[1, 2], ['name', 'caolei'], ['age', 29]]);

//如果有这个键就修改，没有则添加
map.set(键, 值);  

//根据键的名称得到键的值
map.get(键名);

//判断有没有这个键
map.has(键名);

//删除这个键值对
map.delete(键名);

//清空map集合
map.clear();

//得到map集合的长度
map.size;

//遍历map集合
1. for-of 循环，map集合同样也是可迭代的对象

const map = new Map();

for(let item of map){
    console.log(item);
}


2. forEach, value: 值， key: 键，  map: map集合本身

map.forEach((value, key, map) => {
    console.log(value, key, map)
})
```



## 3. WeakSet, WeakMap

#### 3.1 垃圾回收机制

```js
//定义一个变量，赋值为一个对象，在内存中obj变量存储的其实是这个对象的地址

let obj = {
    name: 'caolei',
    age: 20
}

obj  =>  这个对象的地址

//我定义了一个set集合，把这个对象保存到set集合中，这个时候set集合也保存了这个对象的地址
//所以有两个地方保存了这个对象的地址
const set = new Set();
set.add(obj);

//当我把这个变量置为null的时候，垃圾回收机制本应该回收掉这个对象，但是因为set集合中还保存着这个对象的地址
//还是可以通过set集合来访问到的，所以垃圾回收机制并不会回收掉这个对象
obj = null

//也就是说普通的Set和Map集合会影响垃圾回收机制
```



#### 3.2 WeakSet

1. WeakSet的功能几乎和set功能类似，不同的是

   不同点： 
1. **它内部存储的对象地址不会影响垃圾回收机制**
2. **只能添加对象**
3. 不能遍历(不是可迭代对象)，没有size属性，没有forEach方法(因为不会影响垃圾回收机制，里面存储的东西随时有可能被回收，所以记录它的size属性没有任何意义)
4. 它可以观察哪些对象被垃圾回收了

```js
let obj = {
    name: 'caolei',
    age: 20
}

const weakset = new WeakSet();
weakset.add(obj);

obj = null;


console.log(weakset);

//当我打印的时候竟然还保存着obj这个对象，虽然我知道垃圾回收不是瞬发，但是我过了一段时间再打印还是有obj这个对象
//我百思不得其解呀，我都快要怀疑自己了，为什么老师打印的就没有呢？
```



#### 3.3 WeakMap

1. 类似于map集合

​    不同点：
1. **他的键只能是对象**
2. **他的键存储的地址不会影响垃圾回收**
3. 不能遍历(不是可迭代的对象)，没有size属性，没有forEach方法

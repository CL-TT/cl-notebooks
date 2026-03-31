# Set集合详解

## 1.set集合的基本概念

1. **set集合是用来保存多个数据，但是不保存重复的数据**，set集合本身也是一个可迭代的对象



## 2.Set集合的基本使用

```js
//创建一个没有任何内容的集合
const s1 = new Set();

//创建一个具有初始内容的set集合，内容来自于可迭代的对象每一次迭代的结果
const s2 = new Set(iterable);

//s3的结果 1, 2, 3, 4,   不会有重复的结果
const s3 = new Set([1, 2, 2, 3, 4]); 

s1.add(1);   //添加一个数据到set集合末尾，如果数据已经存在则不作任何操作

s1.has();   //判断set集合中是否有此值

s1.delete();  //删除匹配的数据

s1.clear();   //清空整个set集合

s1.size();   //获取集合的长度，只读属性，无法重新赋值
```



## 3.遍历set集合

```js
1. 使用for-of循环来遍历

2. 使用forEach, set集合中不存在下标信息， 所以forEach的第二个参数和第一个参数是一致的

const s = new Set([1, 2, 3]);

for(let item of s){
    console.log(item);
}

s.forEach((item, index, self) => {
    console.log(item);
    console.log(index);
    console.log(self);
})
```



## 4.set集合的去重应用

```js
//数组去重
const s = new Set([1, 1, 2, 3, 3, 4]);

const temp = [...s];   //[1, 2, 3, 4]


//字符串去重
const s1 = new Set('aabbcddee');

const temp = [...s1];  //变成数组

const newStr = temp.join('');  //abcde

```


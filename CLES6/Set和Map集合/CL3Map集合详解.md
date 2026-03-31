# Map集合详解

## 1.Map集合的基本理解

1. map集合专门用于存储多个键值对数据，数据集合的特点就是，键名一定不会重复

2. 以前用对象来保存键值对数据的缺点

   2.1 键名只能是字符串

   2.2 获取数据的数量不方便

   2.3 键名容易跟原型上的起冲突



## 2.Map集合的基本使用

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
```



## 3.如何遍历map集合

```js
1. for-of 循环

const map = new Map();

for(let item of map){
    console.log(item);
}


2. forEach

map.forEach((value, key, map) => {
    
})
```


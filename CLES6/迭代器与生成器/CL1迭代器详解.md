# 迭代器详解

## 1.迭代器的基本理解

1. 迭代器就是把集合中的数据一个一个的取出来，**最重要的是他不一定要取完集合中的数据**，**并且你是跟这个迭代器打交道，而不是跟数据集合打交道**
2. 和遍历的区别就是，遍历一定是要取完集合中的数据的，并且是直接和集合打交道的，比如我要到一个仓库去取商品， 遍历就是商品需要你本人去取并且要取完所有商品，迭代器就是你雇了一个仓库管理员，你让管理员去取商品，你根本就不关心商品是从哪里取的，想取多少就取多少



## 2.成为迭代器的条件

1. 一般迭代器都是一个对象
2. 这个对象里面有一个next属性，这个属性的值是一个函数
3. 这个函数必须返回一个对象，这个对象中有两个属性，一个value, 一个done



## 3.迭代器演示

```js
const arr = [1, 2, 3, 4];

const iterator = {
    index: 0,
    next(){
        return {
            value: arr[this.index++],
            done: this.index > arr.length
        }
    }
}

let data = iterator.next();

while(!data.done){
    console.log(data.value);
    
    data = iterator.next();
}
```



## 4.可迭代的协议与for-of循环

1. ES6规定，**如果一个对象具有知名符号属性，Symbol.iterator,并且属性值是一个迭代器创建函数，则该对象是可迭代的(iterator)**

2. for-of 循环，只有可迭代的对象或者数组才可以使用for of循环

   for-of循环的原理就是利用迭代器调用next方法，取出value的值

3. 展开运算符可以作用于可迭代的对象，这样就可以轻松的将可迭代对象转换为数组

4. 数组是可以使用for-of循环的

```js
//这个obj必须是可迭代的对象

let arr = [...obj]
```

```js
//把一个普通对象变成可迭代对象

const obj = {
    name: 'caolei',
    age: 20,
    [Symbol.iterator](){
        return {
            next: () => {
                let index = 0;
                let keys = Object.keys(obj);  //获取所有的属性名
                const result = {
                    value: this[keys[index]] ,
                    done: index > keys.length
                }
                index++;
                return result;
            }
        }
    }
}
```






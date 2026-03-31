# Array中的一些静态方法

## 1. Array.from()  把伪数组转化成真正的数组

```javascript
function add(a, b, c){
    console.log(Array.from(arguments));   //[1, 2, 3]
}

add(1, 2, 3);
```



## 2. Array.isArray()  判断一个对象是不是一个数组

```javascript
let arr = [];

let obj =  {};

Array.isArray(arr);   //true

Array.isArray(obj);   //false
```





## 3.Array.of()  创建一个数组

```javascript
let arr = Array.of(1, 2, 3);

console.log(arr);    //[1, 2, 3]
```


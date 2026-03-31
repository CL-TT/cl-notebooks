# 函数的本质

## 1.函数的本质

1.函数的本质就是对象，所以函数中可以拥有各种属性

2.所有的函数，都是通过```new Function```创建。

3.函数可分为普通函数和构造函数

4.而所有的对象又是通过   new 构造函数() 创造的

5.流程

在js引擎中他就已经创建了这个 Function这个构造函数

所以其他的函数可以通过 new Function()来创建，包括构造函数

而对象就可以通过这个 new 构造函数() 创造的



```javascript
//这是一般创建函数的写法
function add(a, b){
    return a + b;
}
add(1, 2)   //会打印3

//通过new Function来创建的
let add2 = new Function('a', 'b', 'return a + b');
add2(2, 3);   //会打印5
```


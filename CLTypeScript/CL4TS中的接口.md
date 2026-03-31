# TS中的接口

TypeScript中的接口是一种扩展类型，它是用来约束类，对象，函数的标准，或者说是用来描述他们是什么样子的，和类型别名一样，接口不出现在编译结果中



## 1. 接口的基本使用

```ts
1. 用接口来约束对象

interface User {
    name: string,
    age: number
}

let user:User = {
    name: 'cl',
    age: 20
}

2. 用接口来约束函数

interface fun {
    (n: number): boolean
}


3. 接口和类型别名（type）有什么区别

大体上是一致的，主要的区别在于类的约束上，还有一些区别在于，接口可以继承接口，也可以继承类，类型别名不行

interface A {
    name: string
}

interface B extends A {
    age: number
}

如果类型别名想达到上面的效果，可以交叉类型 &
    
type A = {
    name: string
}

type B = {
    age: number
} & A

子接口不能覆盖父接口的成员， 交叉类型会把相同成员的类型进行交叉

```



## 2. 修饰符readonly

```ts
interface A {
   readonly id: string
}

那么这个id属性除了第一次赋值，就不能修改了
```


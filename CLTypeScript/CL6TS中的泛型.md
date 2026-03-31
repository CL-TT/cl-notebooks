# TS中的泛型

泛型相当于是一个类型变量，在定义时，无法知道具体的类型，可以用该变量来代替，只有在调用的时候，才能确定它的类型

## 1. 基本使用

```ts
1. 在函数中使用

function take<T>(arr: T[]): T[] {
    const newArr: T[] = []
    
    return newArr
}

take<number>([1, 2, 3])




2. 泛型约束
interface hasName {
    name: string
}

// 表明这个泛型上必须有name这个属性

function take<T extends hasName>() {
    
}
```


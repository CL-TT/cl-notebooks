# 基本的类型检查

## 1. 基本数据类型检查

```js
// 有时候不用写，他会自动推断出来

let num:number = 10  // 定义一个number类型的变量,后面的以此类推

let str:string = '字符串'

let flag:boolean = true

let array:number[] = [1, 2, 3] // 定义一个数组，数组的每一项必须是数字

let obj:object = {}

// 注意：null和undefined是所有其他类型的子类型，他们可以赋值给其他类型，但是可以通过在配置文件中添加strictNullChecks:true可以获得更加严格的空类型检查，null和undefined就只能赋给本身
```



## 2. 联合数据类型检查

```js
就是一个数据可以是多个类型

//定义一个数据，既可以是字符串类型也可以是数字类型
let a:string|number = ''
```



## 3. 其他类型检查

```js
1. void: 通常用于约束数据的返回值，表示该函数没有任何返回值

function add():void {}

2. never: 通常用于约束函数的返回值，表示该函数永远不可能结束

3. 字面量类型约束

let a: 'A';  // 表示这个变量a的值只能写成'A'

4. Tuple(元祖类型约束): 一个固定长度的数组，并且数组的每一项的类型都确定

let arr:[number, string] = [2, 'str']

5. any类型的约束: 可以绕过类型检查，因此any类型的数据可以赋值给任意类型，不建议这么做
```



## 4. 类型别名

```js
1. 对已知的类型进行命名 (为了防止冗余的代码)

type 类型别名 = xxx

type user = {
    name: string,
    age: number
}
```



## 5. 函数的相关约束

```js
1. 函数重载：在函数实现之前，对函数的多种情况进行声明

// 这个函数的两个参数可以是字符串也可以是数字，返回值类型也可以是字符串或者是数字
function addNum(a: string|number, b: string|number): string|number{}

2. 可选参数：可以在某些参数后面加上问号，表示该参数可以不用传递

function temp(a: number, b: number, c?: number){}
temp(1, 2)
```



## 6. 扩展类型

```js
1. 枚举

$1. 为什么会出现枚举？

1. 因为字面量类型会把逻辑含义和真实的值产生混淆，这样会造成，当有大量代码改动一处，就要处处改动

enum 枚举的变量名 {
  枚举的字段：枚举的值 
}

注意：1.枚举的值只能用数字或者字符串
     2. 当枚举的值是数字时，它的值是自动递增的，当第一个值没有值时，默认从0开始


//这样就把逻辑含义和真实的值区分开
enum Gender{
  male = "男",
  female = "女"
}

let str: Gender = Gender.male;
```


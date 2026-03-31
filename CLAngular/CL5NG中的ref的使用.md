# NG中的ref的使用

## 1. ref的使用

1. 目的就是可以获取DOM元素， 以及他身上的属性，和方法
2. 这个类似于vue中的ref
3. #input     ref-input

```html
有两种写法


// 1. #input

<input #input value="nihao">

// 2. ref-input(自己起的变量名称)

<input ref-input value="hahah">

<button (click)="handleClick(input)">
    获取input元素的value值
</button>

<script>
    handleClick = () => {
        console.log(input.value) // 可以获取input元素的value值
    }
</script>
```



## 2. 安全导航符

```html
people?.name

如果你不确定people变量上是否有name属性， 可以用安全导航符， 这样在ng中就不会报错
```


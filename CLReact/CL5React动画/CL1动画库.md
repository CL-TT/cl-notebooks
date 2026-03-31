# 动画库react-transition-group

## 1. Transition

```react
<Transition in={}>
    {
        (state) => {
            return <div>haah</div>
        }
    }
</Transition>

1. in的值是布尔值, 用来控制状态

2. 一共有四种状态  entering, entered, exiting, exited

3. 可以根据这四种状态来设置css样式
```



## 2. CSSTransition

### 1.当进入时

1. 为CSSTransition内部的根元素添加样式， enter
2. 在下一帧(enter样式已经完全应用到了元素)立即为该元素添加样式enter-active
3. 当timeout结束后，去掉之前的样式，添加样式enter-done

### 2. 当退出时

1. 为CSSTransition内部的根元素添加样式exit
2. 在下一帧（exit样式已经完全作用到元素之上之后）立即为该元素添加样式exit-active
3. 当timeout结束之后，去掉之前的样式， 添加样式exit-done



```html
可以设置className属性， 指定类样式的名称

1. 字符串： 为类样式添加前缀
2. 对象： 为每一个类样式指定具体的名称（非前缀）

<CSSTransition></CSSTransition>
```





## 3. SwitchTransition

1. 用于有秩序的切换内部组件





## 4. TransitionGroup

1. 该组件的children，接受多个Transition或者CSSTransition组件，该组件用于根据这些子组件的key值，控制他们的进入或者退出状态

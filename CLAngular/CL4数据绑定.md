# 数据绑定

## 1. 属性绑定

1. 属性绑定是用 [ class ] = "myClass"

```html
<div [class]="myClass">
    
</div>

<script>
    myClass = 'my-wrap'
</script>
```



## 2. 事件绑定

1. 事件绑定用 (click)="handleClick()"    这个方法必须要用括号

```html
<div>
    <button (click)="handleClick()">事件触发</button>
    <button (click)="handleClick2($event)">事件触发， 传事件参数</button>
</div>

<script>
    const handleClick = () => {
        console.log('aaa')
    }
    
    const handleClick2 = (e) => {
        // e是事件参数
        console.log(e)
    }
</script>
```



## 3. 自定义事件

```html
// 父组件
<div>
    <h4>{{ count }}</h4>
    <button (click)="handleClick()">父组件的增加方法</button>
    
    <hr>
    // 子组件
    <child (emitClick)="handleClick()"></child>
</div>

<script>
    count = 0
    
    handleClick = () => {
        this.count ++;
    }
</script>

// 子组件
<div>
    <button (click)="handleClick()">子组件的增加方法</button>
</div>

<script>
    import { EventEmmiter, Output } from 'angular/core'
    
    @Output() emitClick = new EventEmmiter()
    
    handleClick = () => {
        // 如果要传参数的话， emit('参数')，  在父组件就需要使用$event来接收一下
        this.emitClick.emit();
    }
</script>

```


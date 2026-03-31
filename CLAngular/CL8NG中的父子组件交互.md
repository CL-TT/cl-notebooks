# NG中的父子组件交互

## 1.使用Input Ouput EventEmitter来交互数据

```html
1. 这种方式非常像vue中的props 和 emit

<app-child [name]="name" (changeName)="changeName()"></app-child>

<script>
    //父组件
    name = 'caolei'
    
    changeName(){
        this.name = 'ckkk'
    }
</script>


// 子组件的dom结构
<div>{{ name }}</div>
<button (click)="cName()">改变父组件的name值</button>

<script>    
    //子组件, 接收传进来的数据
    @Input() name!: string;
    
    @Output() changeName = new EventEmitter()
    
    cName(){
        this.changeName.emit()
    }
</script>
```





## 2. 使用ref

```html
<app-child #child></app-child>


1. 可以通过这个child来调用子组件的属性和方法
```





## 3. @viewChild

```html
<script>
    // 父组件
    import { ViewChild } from '@/angular/core'
    
    @ViewChild(子组件的名称)
    
    private child: 子组件的类型
    
    useChild(){
        this.child.属性|方法()
    }
</script>
```


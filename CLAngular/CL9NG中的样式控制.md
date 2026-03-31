# NG中的样式控制

## 1. 样式的几种写法

```html
<script>
    @Component({
      selector: 'app-ng-parent-child',
      templateUrl: './ng-parent-child.component.html',
      styleUrls: ['./ng-parent-child.component.less']
    })
    export class NgParentChildComponent implements OnInit {

      constructor() { }

      ngOnInit(): void {
      }
    }
</script>
```

1. 样式可以通过 styleUrls: [ '引入样式文件' ]
2. 还可以定义styles: [ 直接写样式 ]
3. 可以在HTML模板中加入style标签，在style标签中写样式
4. 可以在HTML模板中使用link标签， 通过link引入外部的样式
5. 可以在样式文件中通过 @import url(' 样式文件 ')



## 2. 控制试图的封装模式

1. 控制组件的样式会不会对外产生影响， 或者外面的样式会不会对组件产生影响

2. 有三种方式

   2.1 **原生(Native):** Native模式使用浏览器原生的shadow DOM实现来为组件的宿主元素附加上一个shadow DOM，组件的样式被包裹在这个shadow DOM中， **不进不出，没有样式能进来， 也没有样式能出去**

   2.2  **仿真(Emulated):**  仿真模式通过预处理(并改名)css代码来模拟shadow DOM的行为， 以达到把css样式局限于组件视图中的目的，**只进不出， 全局样式能进来， 组件样式出不去**    默认应该是仿真模式

   2.3 **None(无): 能进能出， 全局样式能进来， 组件样式也能出去**

```html
@Component({
   selector: 'app-ng-parent-child',
   templateUrl: './ng-parent-child.component.html',
   styleUrls: ['./ng-parent-child.component.less'],
   //控制视图的封装模式
   encapsulation: ViewEncapsulation.Native
})
```



## 3. NG独有的选择器

1. :host    能以宿主元素为目标的唯一方式， 宿主不是组件自身模板的一部分，而是父组件模板的一部分， 指的是他的父元素。

   :host(.active)

2. :host-context   他在当前组件宿主元素的祖先节点中查找css类，直到文档的根节点为止
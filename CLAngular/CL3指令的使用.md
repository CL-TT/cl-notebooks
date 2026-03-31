# 指令的使用

## 1. if指令

```html
1. *ngIf 代表if指令

2. if else的使用

<ng-container *ngIf="flag; else elseTemplate">
    <div>
        条件成立
    </div>
</ng-container>
<ng-template #elseTemplate>
    <div>
        条件不成立
    </div>
</ng-template>
```



## 2. for指令

```html
1. *ngFor 代表for指令

const list = ['篮球', '足球', '羽毛球', '排球']

const list = [
  { id: 1, name: '张三' },
  { id: 2, name: '李四' },
  { id: 3, name: '王五' },
]

<ul>
    <li *ngFor="let item of list">{{ item }}</li>
</ul>

index是内置的索引标量， odd是，是否是偶数行是一个布尔值， trackBy的作用类似于vue中的key值， trackById是一个函数


/**
* index是索引， item是遍历的数组的每一项
**/
trackById(index, item){
  return index

  return item.id
}

<ul>
    <li *ngFor="let item of list2; let i = index; let odd = odd; trackBy: trackById"></li>
</ul>
```



## 3. switch指令

```html
1. [switch]   *ngSwitchCase   *ngSwitchDefault

level = 'A

<span [ngSwitch]="level">
<p *ngSwitchCase="'A'">
  太棒了！
</p>
<p *ngSwitchCase="'B'">
  继续加油哦！
</p>
<p *ngSwitchDefault>
  不及格哦！
</p>
</span>
```



## 4. style指令

```html
1. [style.color]="'red'"  [ngStyle]="{}"

两种书写方式

<div [style.color]="'blue'">style指令</div>
<div [ngStyle]="{'color': 'red'}">style指令</div>
```



## 5. class指令

```html
1. [class]="'wrap'"   [ngClass]="{}"

<div [class]="'wrap'">哈哈class</div>
<div [ngClass]="{'wrap': true}">ng-class</div>
```


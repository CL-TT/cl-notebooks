# NG中的自定义指令

## 1. 自定义指令的用法

1. 需要新建一个direct.component.ts文件， 文件如下
2. 指令的名称可以是， #app, .app, [app]
3. 指令文件需要在app.module文件中声明
4. HostListener是可以监听被指令绑定的元素的Dom操作的

```ts
import { Directive, ElementRef, Input, OnInit, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighLight]',
})
export class AppHighLightDirective implements OnInit { 
  @Input() appHighLight?: string

  constructor (private e: ElementRef) {
    // e.nativeElement 指的就是被指令绑定的元素
  }

  ngOnInit(): void {
    if (!this.appHighLight) {
      this.e.nativeElement.style.background = 'red'
    }else {
      this.e.nativeElement.style.background = this.appHighLight
    }
  }

  //监听一些DOM事件
  @HostListener('mouseenter') mouseEnter(){
    this.e.nativeElement.style.background = 'green'
  }
}
```



```html
<div appHighLight>背景变色</div>
```


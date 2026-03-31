# 创建组件



## 1.手动创建组件

1. 需要在app文件夹下， 创建一个组件文件夹， 比如说这个组件叫hello-world
2. **一定要注意引用组件的时候，要使用selector的名称**

```ts
// 最简单的一个组件模板

import { Component } from '@angular/core';

@Component({
  selector: 'app-name', // 组件名称，使用这个组件的时候，用的是这个名称
  templateUrl: './hello-world.html', //html模板
  styleUrls: ['./hello-world.less'] //模板样式
})

export class NameComponent {
  constructor() { }

  title = 'HelloWorld'
}
```



2. 需要在app.module.ts中注册这个组件，才可以使用

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

import { HelloWorld } from './helloWorld/hello-world.component';

@NgModule({
  declarations: [
    AppComponent,
    HelloWorld
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```



## 2. 用命令行的方式创建

```js
ng generate component my-comp
```


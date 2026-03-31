# NG中的服务

## 1. 什么是服务

1. ng中的服务更像是提供一种方式抽取共用类库
2. 比如说一些工具方法， 我们传统的做法就是自己写一个unit工具库， 把相关的方法填充到unit文件里面去， 在需要的地方调用即可。 如果使用angular中的服务， 创建一个unit服务， 然后在任何地方都可以通过依赖注入调用unit里面的方法



## 2. 服务service的使用

#### 1. 创建服务

```ts

import { Injectable } from '@angular/core';
import { IList } from 'src/types/common';

@Injectable({
  providedIn: 'root'
})
export class GetListService {
  list = [
    { name: 'caolei', age: 20 },
    { name: 'nhsjs', age: 30 },
    { name: 'lakusi', age: 40 },
    { name: 'najsu', age: 50 },
  ] 

  getList(): IList[] {
    return this.list;
  } 
}
```



#### 2. 全局注册服务

```ts
import { GetListService } from ''

在providers中注册服务

providers: [ GetListService ]

//还有其他几种挂载方式

//当一个旧的服务不在使用时， 而这个服务的名称又在文件的各个地方使用， 修改起来比较麻烦， 所以可以定义一个新的服务来代替旧的服务
providers: [
    { provide: GetListService, useClass: NewGetListService }
]

//这种方法只改变服务中指定的方法
providers: [
    { provide: GetListService, useValue: { getCount(){} } }
]
```



#### 3. 局部注册服务

```ts
在组件的装饰器中有一个provider属性

@component {
    providers: ''
}
```


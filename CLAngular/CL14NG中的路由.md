# NG中的路由

## 1. 路由的定义，以及嵌套路由

1. 先定义一个app-routing.module.ts文件这里面专门放跟路由相关的代码

```ts
import { NgModule } from '@/angular/core';

import { RouterModule, Routes } from '@/angular/router'

const routes: Routes = [
    {
        path: '',
        component: ''
    },
    {
        path: '',
        component: '',
        children: []
    },
    // 通配符路由
    { path: '**', component:  }
]

@NgModule({
    imports: [ RouterModule.forRoot(routes) ],
    exports: [ RouterModule ]
})

export class AppRoutingModule {}
```

2. 然后再app.module.ts文件里面引入

```ts
import { AppRoutingModule } from ''

在imports里面写入
```



## 2. 路由的重定向

```ts
{ path: '', redirectTo: '/', pathMatch: 'full' }

被重定向的路径不能写component了， 否则会报错
```



## 3. 参数路由

```ts
{ path: 'page/:id' }

import { Router, ActivatedRoute, ParamMap } from '@angular/router';

// 获取路由参数
this.route.snapshot.paramMap.get('id')
```



## 4. 路由跳转

```ts
1. 可以通过a标签上的routerLink属性进行跳转

<a routerLink="page/10">
    
    
2. 通过编程式导航的方式进行跳转

import { Router } from '@/angular/router'

this.router.navigate(['page', '20'])

this.router.navigateByUrl('page/20')
```



## 5. 路由守卫

```ts

```


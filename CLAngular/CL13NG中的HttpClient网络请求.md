# NG中的HttpClient网络请求

## 1. HttpClient简介

1. angular给应用提供了一个http客户端API， 也就是@angular/common/http中的HttpClient服务类
2. 它有简化的错误处理， 请求和响应的拦截机制



## 2. HttpClient的使用

```ts
// 1. 想要使用需要在AppModule中导入HttpClientModule

// 2. 在组件中引入 import { HttpClient } from '@/angular/common/http'

constructor(http: HttpClient){}

onInit(){
    const url = '';
    
    this.http.get(url).subscribe(res => {
        console.log(res)
    })
}
```


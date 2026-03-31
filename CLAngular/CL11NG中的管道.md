# NG中的管道

## 1. 什么是管道

1. 管道相当于vue中的过滤器， 他是一种可以把数据变换成其他形式的一种方式
2. ng中有内置的管道， 当然也可以自定义管道
3. 管道是可以链式调用的



## 2. 管道的使用

#### 2.1 内置管道

```html
<div>
    {{ time | date }}
</div>

1. 日期管道 后面跟着的是参数

time | date: 'yyyy-MM-dd HH:mm:ss'

2. 大小写转换管道

<div>
    {{ data | uppercase }}    小写转大写
    {{ data | lowercase }}    大写转小写
</div>

3. 小数管道,  接收的参数格式为{最少整数位数}.{最少小数位数}-{最多小数位数}
<div>
    {{ data | number }}
    {{ data | number: '1.2-2' }}
</div>

4. 对象序列化
<div>
    {{ {name: 'caolei'} | json }}   => { "name": "caolei" }
</div>


5. 字符串截取
<div>
    {{ 'caolei' : slice: 0:3 }}  =>  cao
</div>
```



#### 2.2 自定义管道

```html
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({name: 'sayPipe'})
export class sayPipe implements PipeTransform {
  transform(value: any): any {
    console.log(value);
    return 'say' + value
  }
}

<div>{{ str | sayPipe }}</div>
```


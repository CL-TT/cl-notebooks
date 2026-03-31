# 其他的路由标签

## 1. Link

1. link是生成一个无刷新的a元素

2. to可以是一个字符串，也可以是一个对象

   #1. pathname: url路径

   #2. search

   #3. hash

   #4. state: 附加的状态信息

3. replace属性  布尔值，  默认为false

4. innerRef: 可以是一个函数，也可以是一个ref对象， 相当于ref转发

```html
import { Link } from 'react-router-dom';

<Link to="路径" >
```



## 2. NavLink

import  { NavLink } from 'react-router-dom'

1. link有的功能它都有

2. 它独有的特点是： 根据当前的地址和链接地址，来决定该链接的样式

3. 可以根据独有的class名称来决定样式

4. 几个属性

   #1. activeClassName: 匹配时使用的类名

   #2. activeStyle: 匹配时使用的内联样式

   #3. exact: 是否精确匹配： 默认为false

   #4. strict: 是否严格匹配最后一个斜杠



## 3. Redirect

1. 属性：

   #1.  to

   #2. from 当匹配到from地址规则时才进行跳转

   #3.  push默认值为false， 表示跳转使用替换的方式

```html
import { Redirect } from 'react-router-dom'

<Redirect to=""></Redirect>
```


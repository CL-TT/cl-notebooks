# 表单元素

## 1. 受控组件与非受控组件

###  1. 受控组件

1. 组件的使用者，有能力完全控制该组件的行为和内容，通常情况下，受控组件往往没有自身的状态，其内容完全收到属性的控制



##  2. 非受控组件

1. 组件的使用者，没有能力控制该组件的行为和内容，组件的行为和内容完全自行控制



## 2. 表单元素

1. **表单元素，默认情况下是非受控组件，一旦设置了表单组件的value属性，则其变为受控组件(单选和多选框需要设置checked属性)**



```react
// 文本元素演示

import React from 'react'

export default function Test(props){
    return (
        <div>
            <input
                type="text"
                value={ props.val }
                onChange = {(e) => {change(e, props)}}
            ></input>
        </div>
    )
}

function change(e, props){
    const newVal = e.target.value;
    
    props.change && props.change(newVal);
}
```


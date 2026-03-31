# 优化表单验证

## 1. 没有优化之前的写法

```js
// 在移动端

submit(){
    if(!this.ruleForm.name){
        this.$toast('xxx不能为空')
        return false
    }
    if(!this.ruleForm.password){
        this.$toast('xxx不能为空')
        return false
    }
}

缺点：

1. 扩展性非常差
2. 如果想要增加一条新的校验规则，要到submit方法内部更改，这样就违反了开放-封闭原则
```



## 2. 使用策略模式优化表单校验

```js
1. 把那些检验逻辑全部都封装成一个策略对象

const strategies = {
    // 是否是空值
    isEmpty: function(value, errorMsg){
        if(value === '' || value === null){
            return errorMsg;
        }
    },
    
    // 是否是正确的手机号
    isMobile: function(value, errorMsg){
        if(!/(^1[3|5|8][0-9]{9}$)/.test(value)){
            return errorMsg;
        }
    }
}

2. 要暴露出去一个验证的方法类

class Validator {
    constructor(){
        this.cache = []
    }
    
    /**
    * 1. value: 字段的值
    * 2. rules: 验证规则
    **/
    add(value, rules = []){
        let self = this;
        
        rules.foreach(item => {
            const strageryName = item.stragery;
            
            let valueArr = Object.values(item);
            
            valueArr.shift(); //删除策略的名称
            
            valueArr.unshift(value); //把要检验的值加入进去
            
            self.catch.push(function(){
                return strageries[strageryName].apply(self, valueArr);
            })
        })
    }
    
    /**
    * 把缓存里面的方法拿出来执行一遍
    **/
    start(){
        const len = this.catch.length;
        
        for(let i = 0; i < len; i++){
            let msg = this.catch[i]();
            
            if(msg){
                return msg;
            }
        }
    }
}
```



```js
// 在页面中使用

function check(){
    const validator = new Validator();
    
    validator.add(this.ruleForm.name, [
        { stragery: 'isNotEmpty', errorMsg: '姓名不能为空' }
    ])
    
    return validator.start();
}


function handleSubmit(){
    const msg = this.check();
    
    if(msg){
        this.$toast(msg);
        
        return;
    }
    
    console.log('检验通过');
}
```


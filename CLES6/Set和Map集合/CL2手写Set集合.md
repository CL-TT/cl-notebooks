# 手写Set集合

```js
class MySet{
    constructor(iterator = []){
        this._arr = [];
        
        //1. 先要判断你传过来的参数是不是可迭代对象
        if(typeof iterator[Symbol.iterator] !== 'function'){
            throw Error(`您传的参数${iterator}不是可迭代对象`);
        }
        
        //遍历可迭代对象
        for(let item of iterator){
            this.add(item)
        }
    }
    
    /**
    * 把数据加入到集合中
    **/
    add(data){
        //先要判断这个数据在集合中是否存在
        if(this.has(data)) return;
        
        this._arr.push(data);
    }
    
    /**
    * 判断这个数据在集合中是否存在
    **/
    has(data){
        for(let i = 0; i < this._arr.length; i++){
            if(this.equal(data, this._arr[i])) return true;
        }
        
        return false
    }
    
    /**
    * 清空这个集合
    **/
    clear(){
        this._arr = [];
    }
    
    /**
    * 删除集合中的某个数据
    **/
    delete(data){
        for(let i = 0; i < this._arr.length; i++){
            if(this.has(data)){
                this._arr.splice(i, 1);
            }
        }
    }
    
    /**
    * 获取集合的长度
    **/
    get size(){
        return this._arr.length;
    }
    
    /**
    * 判断两个数据是否相等
    * 在set集合中 +0 和 -0是相等的
    **/
    equal(data1, data2){
        if(data1 === 0 && data2 === 0) return true;
        
        return Object.is(data1, data2);
    }
}
```


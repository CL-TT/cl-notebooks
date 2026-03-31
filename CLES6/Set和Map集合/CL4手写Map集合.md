# 手写Map集合

```js
class MyMap{
      constructor(iterator = []){
        this._arr = [];

        //先要判断是不是可迭代对象
        if(!this._isIterator(iterator)){
          throw Error(`${iterator}不是可迭代对象`);
          return;
        }

        //还要判断他里面的内容是不是可迭代对象
        for(let item of iterator){
          if(!this._isIterator(item)){
            throw Error(`${item}不是可迭代对象`);
            return;
          }

          //注意这里的子内容不单单是数组，是可迭代的对象
          const iteratorTask = item[Symbol.iterator]();
          const key = iteratorTask.next().value;
          const value = iteratorTask.next().value;

          //把键值对添加到集合中
          this.set(key, value);
        }
      }

      /**
       * 如果键存在就修改值，不存在就加入到集合中
       **/
      set(key, value){
        if(this.has(key)){
          //如果有key值,修改
          this._getData(key).value = value;
        }else{
          //没有就添加
          this._arr.push({
            key,
            value
          })
        }
      }

      /**
       * 判断集合中有没有这个key值
       **/
      has(key){
        for(let item of this._arr){
          if(this.equal(key, item.key)){
            return true;
          }
        }
        return false;
      }

      /**
       * 根据键来获取值
       **/
      get(key){
        const result = this._getData(key);
        if(!result) return;
        return result.value
      }

      /**
       * 清空集合
       **/
      clear(){
        this._arr = [];
      }

      /**
       * 根据键来删除这个值
       **/
      delete(key){
        for(let i = 0; i < this._arr.length; i++){
          if(this.equal(key, this._arr[i].key)){
            this._arr.splice(i, 1);
            return;
          }
        }
      }

      get size(){
        return this._arr.length;
      }

      forEach(callback){
        for(let {key, value} of this._arr){
          callback(value, key, this);
        }
      }

      *[Symbol.iterator](){
        for(let item of this._arr){
          yield item;
        }
      }

      /**
       * 判断这个key值是否存在
       **/
      equal(data1, data2){
        if(data1 === 0 && data2 === 0){
          return true;
        }

        return Object.is(data1, data2);
      }

      /**
       * 获取对象
       **/
      _getData(key){
        for(let item of this._arr){
          if(this.equal(key, item.key)){
            return item;
          }
        }
      }

      /**
       * 判断是不是可迭代对象
       **/
      _isIterator(data){
        return typeof data[Symbol.iterator] === 'function'? true : false 
      }
}
```


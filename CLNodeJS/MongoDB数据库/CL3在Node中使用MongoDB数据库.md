# 在Node中使用MongoDB数据库

## 1.基本流程

```javascript
//1.安装  npm install mongoose --save

//2.引包
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

//3.连接数据库
mongoose.connect('mongodb://localhost/数据库名');

//4.对数据进行约束
var schema = new ({
    name: String,
    age: Number
})

//5.创建一个数据模型
var People = mongoose.model('集合名', schema);

//6.创建一个实例
var caolei = new People({name: 'caolei', age: 20});

//7.数据持久化     相当于增加一个数据
caolei.save(function(error){
    if(error){
        console.log('保存失败');
    }else{
        console.log('保存成功');
    }
})
```

## 2. 数据的增删改查

```javascript
//1.查询数据
Model.find({条件}, function(error){
    if(error){
        
    }else{
        
    }
})

//find是把查询到的数据放入到数据中
//findOne 查询到的数据就是单个的对象
Model.findById()

//2.删除数据
Model.remove()

Model.deleteOne()

Model.deleteMany()

Model.findOneAndRemove()

Model.findByIdAndRemove()

//3.更新数据
Model.findOneAndUpdate()

Model.findByIdAndUpdate()

Model.update()

Model.updateMany()
```


# 模型的创建和同步

## 1.流程

```javascript
//1. sequelize和数据库的连接

const { Sequelize } = require('sequelize');

const sequelize = new Sequelize(数据库名, 用户名, 密码, {
    host: 'localhost',
    dialect: 'mysql',
    logging: null,
});

module.exports = sequelize;


//2. 创建模型

const sequelize = require('./db');

const { DataTypes } = require('sequelize');

const Student = sequelize.define('Student', {
    name: {
        type: DataTypes.STRING,
        allowNull: false
    }
}, {
  createdAt: false,
  updatedAt: false,
  paranoid: true  //不正真删除数据库的数据， 创建一个删除时间的列
})

module.exports = Student;


//3. 模型同步
const sequelize = require('sequelize');

sequelize.sync({
    alter: true
}).then(() => {
    console.log('所有模型同步完成');
})


//4. 外键连接,  程序会自动给你创建外键
const stu = require('./student');

Classes.hasMany(stu);


```


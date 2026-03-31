# 关键字in

1. 判断某个属性在对象中是否存在

   属性名 in 对象

   ```javascript
   var obj = {
       name: 'caolei',
       age: 20,
       sex: 'nan'
   }
   
   'name' in obj     //返回true
   
   
   //这个in可以作为判断数组是否是稀松数组
   let arr = [1, 2, 3, 2, 3];
   
   let target = false;
   
   for (let i = 0; i < arr.length; i++) {
      if (!(i in arr)) {   //只要索引不连续就是稀松数组
        target = true;
       }
    }
    console.log(target);
   ```

   
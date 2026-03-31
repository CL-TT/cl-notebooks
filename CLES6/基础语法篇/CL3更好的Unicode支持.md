# 更好的Unicode支持

## 1.出现的问题

1. 有一些特殊汉字，虽然只有一个字但是他的 str.length = 2 却等于2

2. 原因：

   1. 早期，存储空间有限，Unicode使用16位二进制来存储文字，将一个16位的二进制编码叫做码元，

   2. 后来，技术的发展，Unicode对文字的编码进行了扩展，将某些文字扩展到了32位(占用了2个码元)

      所以某些文字的长度就变成了了2， 将某个文字对应的二进制数叫做码点。



## 2.ES6提供的解决办法

1.  charCodeAt

   这是原本就有的方法 可以获得码元

   ```javascript
   var str = '特殊的字符';    //假如他的长度为2，那么就有两个码元
   
   str.charCodeAt(0);     //取出第一个码元
   str.charCodeAt(1);     //取出第二个码元
   ```

   

2. codePointAt

   ES6新增方法，可以获得码点

   ```javascript
   str.codePointAt(0);    //会获得第一个码元和第二个码元的拼接值，会整个都取出来
   ```

   

### 2.1.判断一个字符是32位还是16位

```javascript
function is32bit(char, index = 0){
    //这个index默认为0
    return char.codePointAt(index) > oxffff;
}
```



### 2.2.利用码点的方式计算一个字符串的真正长度

```javascript
function getRealLenByPoint(str){
    let count = 0;
    for(let i = 0; i < str.length; i++){
        if(is32bit(str, i)){
            i++;   //跳过下一次循环
        }
        count++;
    }
    return count;
}
```


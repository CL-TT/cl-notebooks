# String包装类

## 1.静态成员

1. fromCharCode：通过unicode编码创建字符串

### 2.实例成员

1. length：字符串长度  字符串是一个伪数组

2. charAt：得到指定位置的字符

3. charCodeAt: 得到指定位置的字符的Unicode编码

4. concat：拼接字符串并返回一个新字符串

5. includes：看字符串中是否包含目标字符串

6. endsWith：看字符串是否以目标字符串结尾

7. startsWith： 看字符串是否以目标字符串开头

8. indexOf：找出目标字符串的索引位置， 不存在就返回-1

9. lastIndexOf：找出最有一个目标字符串的索引位置

10. padStart：指定长度的字符串，如果不够就把数据插到头部

11. padEnd：指定长度的字符串，如果不够就把数据插到尾部

12. repeat：把字符串重复几次并返回新的字符串

13. slice：从某个位置取到某个位置；位置可以是负数；

14. substr: 从某个位置开始取，取指定的长度；位置可以是负数；

15. substring：从某个位置取到某个位置；不可以是负数；参数位置是可调换的。

16. toLowerCase ： 字符串转小写

17. toUpperCase：字符串转大写

18. split：分割字符串     返回一个数组
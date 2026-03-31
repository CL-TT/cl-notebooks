# JS篇

```js
1. var obj = { a: 1, b: 2, c: 3 } 做什么操作，能变成 obj = { a: 1, c: 3 }

2. 使用正则表达式验证邮箱格式

3. 请写一个function，能清除字符串前后的空格

4. let a = a; 在运行的时候会出现什么问题，为什么

5. 使用typeof obj = 'object', 来确定变量obj是一个对象有什么缺陷， 这个缺陷如何避免

6. 下面的代码输出到控制台的是什么， 为什么
(function(){
    var a = b = 3;
})();

console.log("a defined?" + (typeof a !== 'undefined'));
console.log("b defined?" + (typeof b !== 'undefined'))


7. 
function a(){
    y = function(){
        x = 2;
    };
    return function(){
        var x = 3;
        y();
        console.log(this.x)
    }.apply(this, arguments);
}
a();
问：控制台输出什么


8.
var length = 10;
function fn(){
    console.log(this.length);
}
var obj = {
    length: 5,
    method: function(fn){
        fn();
        arguments[0]();
    }
}
obj.method(fn, 1);
问：控制台输出什么


9.
function fun(n, o){
    console.log(o)
    return {
        fun: function(m){
            return fun(m, n);
        }
    }
}
var a = fun(0);
a.fun(1);
a.fun(2);
a.fun(3);

var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1);
c.fun(2);
c.fun(3);
问： 控制台输出什么


10.
var dad = {};
var son = {};
function show(){
    return this;
}
var newShow = show.bind(dad);
var newshow1 = newShow.bind(son);

console.log(newShow() == dad);
console.log(newShow1() == son);
问：输出什么


11. 
console.log(1 + "2" + "2");
console.log(1 + +"2" + "2");
console.log(1 + -"2" + "2");
console.log(+"1" + "1" + "2");
console.log("A" - "B" + "2");
console.log("A" - "B" + 2);
问： 控制台输出什么


12. 使用js将右边字符串转为驼峰形式  'border-left-color' =>  'borderLeftColor'


13. 你如何获取浏览器中URL中查询字符串的参数(就是?号后面的部分)，并将参数组成一个对象 url为http://www.runoob.com/jquery/misc-trim.html?channelid=12333&name=xiaoming&age=23，
最后返回给我的结果就是{ channelid: 12333, name: xiaoming, age: 23 }


14.
var count = 10;
function add(){
    var count = 0;
    return function(){
        count += 1;
        console.log(count);
    }
}
var s = add();
s();  //输出什么
s();  //输出什么


15. window.onload和document.ready的区别


16. 
console.log("0 || 1 =" + (0 || 1));
console.log("0 || 2 =" + (1 || 2));
console.log("0 && 1 ="+ (0 && 1));
console.log("1 && 2 ="+ (1 && 2));
问：控制台输出什么


17. 去除字符串中的字母， "a12f3ba456h345d"


18. 写一个函数，查找出var a = "aaaabbbddbbbbccc"连续出现次数最多的字符，将这个字符和它的出现的次数打印出来


19.
function makeNoSense(x){
    this.x = x;
}
makeNoSense(5);
console.log(x);
function test(){
    this.x = 1;
    alert(this.x);
}
test();
问：控制台输出什么


20.
console.log(0.1 + 0.2);
console.log(0.1 + 0.2 == 0.3);
问：下面代码输出什么，解释你的答案，如何避免这个问题


21.
(function(x){
    return (function(y){
        console.log(x);
    })(2)
})(1);
问：控制台输出什么


22.
var b = 1;
function outer(){
    var b = 2;
    function inner(){
        b++;
        var b = 3;
        console.log(b)
    }
    inner();
}
outer();
问： 控制台输出什么


23. 
for(let i = 0; i < 5; i++){
    setTimeout(function(){
        console.log(i);
    }, i * 1000);
}
问：控制台输出什么


24. 
var a = 1;
if (function b(){}){
    a += typeof(b);
}
console.log(a);
问：控制台输出什么


25. 函数形参与arguments的关系，下面代码运行结果是什么
function course(name, age){
    console.log(age);
    console.log(arguments[1]);
    arguments[1] = 2;
    console.log(age);
    console.log(arguments[1]);
}


26.
var a = 0, b = 0;
function A(a){
    A = function(b){
        alert(a + b++);
    }
    alert(a++);
}
A(1);
A(2);
问： 输出什么



27.
var x = 1;
var y = 2;
function show(){
    var x = 3;
    return {
        x: x,
        fun: function(a, b){
            x = a + b;
        }
    }
}
var obj = show();
obj.fun(x, y);
console.log(obj.x);
console.log(x);


28. 下面的代码返回什么?
(true + false) > 2 + true
```


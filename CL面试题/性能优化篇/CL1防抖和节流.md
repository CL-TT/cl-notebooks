# 防抖和节流

## 1. 防抖

1. 防抖的基本理解： 一个函数我只希望他在n秒后执行一次， 如果他在等待的n秒内多次被执行，那么我将重新计时，如果n秒过后都没有再次触发这个函数，那么就可以执行这个函数。一定是等到某个动作完成后再执行。
2. delay 2s,   频繁的触发这个函数， 这次触发的时间与上一次触发的时间间隔小于(delay  2s),  都不会触发这个事件

3. 多用于搜索， 或者调整窗口大小resize次数过于频繁

```html
<div>
    <input type="text" id="text">
</div>

<script>
    const input = document.querySelector('#text');
    
    function input(){
        console.log('a');
    }
    
    input.addEventListener('input', Debounce(input, 1000));
    
    /**
    * func: 传入一个处理函数
    * delay: 延时多长时间执行
    **/
    function Debounce(func, delay){
        let timer = null;
        
        return function (...args){
            clearTimeout(timer);
            
            timer = setTimeout(() => {
                func.call(this, ...args);
            }, delay);
        }
    }
</script>

```



## 2. 节流

1. 节流的基本理解： 有一个时间量wait 假如为2s，在这2s之内不管你触发了多少次事件都不会执行， 只要过了2s就会执行一次  ，所以到了一定的时间一定会执行一次
2. scroll事件， 也可用于搜索

```javascript
//第一种写法   时间戳是毫秒
function Throttle(func, wait){
    let lastTime = new Date().getTime();
    
    return function(...args){
        let nowTime = new Date().getTime(); 
        
        if(nowTime - lastTime >= wait * 1000){
            func.apply(this, args);
            
            lastTime = nowTime;
        }
    }
}

//第二种写法
function Throttle(func, wait){
    let timer = null;
    
    return function(...args){
        if(timer) return;
        
        timer = setTimeout(() => {
            func.apply(this, args);
            
            timer = null;  //不要忘记把timer重新置为null
        }, wait)
    }
}




```


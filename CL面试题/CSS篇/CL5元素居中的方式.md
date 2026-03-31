# 元素居中的方式

## 1.知道宽高

```css
1.知道宽高
div {
   width: 200px;
   height: 200px;
   position: absolute;
   left: 50%;
   top: 50%;
   margin-left: -100px;
   margin-top: -100px; 
}
```



## 2.不知道宽高

```css
1.第一种
div{
   position: absolute;
   left: 50%;
   top: 50%;
   transform: translate(-50% -50%); 
}

2.第二种 这种知道宽高也是适用的
div{
   position: absolute;
   margin: auto;
   left: 0;
   right: 0;
   top: 0;
   bottom: 0; 
}

3.第三种利用flex布局
这个元素比需要有父元素，并且父元素要有宽高
设置父元素的  justify-content: center   align-items: center
```



```html
1. 利用display: table-cell属性

这个必须借助他有父元素，并且希望子元素是行级元素，或者行级块元素
<div class="wrap">
    <div>
        
    </div>
</div>

.wrap{
  display: table-cell
  vertical-align: middle
  text-align: center
}

```


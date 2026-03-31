# CSS设置链接样式

## 1. a:link,a:visited,a:hover,a:active 分别是什么意思?

1. link:连接平常的状态

2. visited:连接被访问过之后

3. hover:鼠标放到连接上的时候

4. active:连接被按下的时候



## 2.链接的规则

正确顺序：“爱恨原则”（LoVe/HAte），即四种伪类的首字母:LVHA。再重复一遍正确的顺序：a:link、a:visited、a:hover、a:active .



因为当鼠标经过未访问的链接，会同时拥有a:link、a:hover两种属性，a:link离它最近，所以它优先满足a:link，而放弃a:hover的重复定义。当鼠标经过已经访问过的链接，会同时拥有a:visited、a:hover两种属性，a:visited离它最近，所以它优先满足a:visited，而放弃a:hover的重复定义。究其原因，是css的就近原则“惹的祸”。
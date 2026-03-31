# 为什么有ref了还需要reactive

ref和reactive都是可以得到响应式数据的，reactive能做到的事情，ref也能做，为什么还要搞出两个api呢

原理：vue3中的响应式底层是通过ProxyAPI来实现的，**但是这个ProxyAPI只能对对象进行拦截，无法对原始值进行拦截**，那么这里就会产生一个问题，如果用户想要把一个原始值转换成响应式数据怎么办？

两种方案：

1. 让用户自己处理，用户需要将自己想要转换的原始值包装为对象，然后在使用reactive API
2. 框架层面来处理，多提供一个API，这个API来帮助用户简化操作，将原始值也能转换成响应式数据

ref的背后其实也调用了reactive API 

原始值：Object.defineProperty

复杂值：reactive API
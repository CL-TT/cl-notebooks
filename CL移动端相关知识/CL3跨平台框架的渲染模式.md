# 跨平台框架的渲染模式

## 1. uni-app

uni-app是通过webview渲染（渲染层） +  js引擎（逻辑层）

webview渲染：通过webview的组件加载HTML，CSS，JS，依赖web引擎

js引擎：uni-app的js引擎在安卓上是v8，ios上是JavaScriptCore 

这种方式性能是最低的

uni-app也支持原生渲染（nvue）



## 2. React-native

react-native是原生的渲染方式，也可以采用webview的渲染方式（react-native-webview）

原生渲染：使用操作系统提供的原生UI组件进行绘制

js引擎：默认使用Hermes引擎，之前有V8，JavaScriptCore

**这种方式要比webview的性能好，但是渲染层和逻辑层不在一起，也会有一定的性能损耗**



### 3. flutter

flutter是自绘渲染，也可以采用webview渲染方式（flutter_webview_plugin）

自绘渲染：不使用原生的UI组件，而是通过Skia 2D图形引擎直接在屏幕上绘制UI，因为没有JS引擎，所以逻辑采用Dart语言

1. Dart代码运行在flutter引擎中，生成UI组件
2. flutter引擎使用Skia直接绘制UI
3. 渲染完成之后，flutter直接输出到屏幕，绕过了原生UI系统

**这种方式逻辑层和渲染层是在一起的，性能很高，但是自绘渲染还是会有一定的性能损耗**
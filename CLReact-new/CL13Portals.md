# Portals的使用

## 1. Portals解决什么样的问题

Portals的作用就是让，**组件的Dom节点可以渲染到页面任意指定的地方，但组件逻辑，事件，状态依然停留在原来的父组件中**

解决的场景，比如模态框，提示框，全局加载的动画，这些组件需要挂载到body的最外层，防止被父元素的样式影响



## 2. Portals的使用

```jsx
function App() {
  return (
    <div id="root">
      <div onClick="">
         // 具体内容
         // 实际modal组件在这里面
         // 我点击了modal组件，父组件的click方法会被触发
      </div>
          
      <div id="modal">
        我希望模态框挂载到这个div下面， 但是经过Portal的处理，被挂载到这里
      </div>
    </div>
  )
}

import { createPortal } from 'react-dom';
function Modal() {
    // 第一个参数要渲染的内容
    // 第二个参数要挂载到哪个dom节点上
    return createPortal(<div></div>, document.getElementById('modal'))
}


事件冒泡已经会在原来的父组件中生效
```


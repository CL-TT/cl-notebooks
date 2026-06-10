# Recoil状态管理库（官方推出的）

## 1. 核心概念

### atom

atom被翻译为原子，表示的就是一个可变的共享状态的最小单位，每一个atom都会有一个唯一的标识和一个默认值,那么当这个状态数据发生变化时，所有订阅这个原子的组件就会重新渲染

```react
import { atom } from 'recoil'

atom({
    key: 'count',
    default: 0
})
```



### selector

这是一个派生状态，有点类似于计算属性，同样的selector里面依赖的atom发生改变，那么所有订阅这个selector的组件都会重新渲染

```react
import { selector } from 'recoil'

selector({
    key: 'total',
    get: ({ get }) => {
        const count = get(count)   // 获取到这个状态数据
        
        return count * 2
    }
})
```



## 2. 需要了解的hook

useRecoilState      有点类似于useState

useRecoilValue      获取值

usesetRecoilState   获取改变值的方法

```react
import { atom, useRecoilState } from 'recoil'

const countAtom = atom({
    key: 'count',
    default: 0
})

const [ count, setCount ] = useRecoilState(countAtom)

const count = useRecoilValue(countAtom)

const setCount = useSetRecoilState(countAtom)
```



## 3. 使用

```react
在最外层组件还需要加一个组件

import { RecoilRoot } from 'recoil'

<RecoilRoot>
  <App />    
</RecoilRoot>
```


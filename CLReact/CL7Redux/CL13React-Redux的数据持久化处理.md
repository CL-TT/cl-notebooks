# react-redux的数据持久化处理

## 1. 安装依赖

yarn add redux-persist     react-redux    @reduxjs/toolkit



## 2. 最佳实践

```react
store.js   // store.js文件的修改

import { configureStore, combineReducers } from '@reduxjs/toolkit'

import { persistStore, persistReducer } from 'redux-persist'

import storage from 'redux-persist/lib/storage' // 默认使用localStorage作为存储引擎

// 整合所有的reducer
const reducer = combineReducers({
    user: userReducer
})

// 配置持久化设置
const persistConfig = {
  key: 'root', // 存储的键名
  storage // 持久化存储引擎
  // 可选的配置项，如白名单、黑名单等 选其一就好了
  // blacklist:['test'], // 只有 test 不会被缓存
  // whitelist: ["test"], // 只有 test 会被缓存
}

const persistedReducer = persistReducer(persistConfig, reducer)

// 创建store
export const store = configureStore({
  reducer: persistedReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: false // 关闭默认的序列化检查//关闭严格模式
    })
})

// 创建持久的仓库
export const persistor = persistStore(store)
```

```react
index.js     //  仓库的引用被修改

// 同时引入仓库和持久化仓库
import { store, persistor } from './redux'

import { PersistGate } from 'redux-persist/integration/react'

import { Provider } from 'react-redux'

<Provider store={ store }>
    <PersistGate loading={ null } persistor={ persistor }>
       <App />
    </PersistGate>
</Provider>
```


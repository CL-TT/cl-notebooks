# 工具库的使用

## 1. React-Navigation

```react
react-navogation导航库

1. 安装

npm i @react-navigation/native
expo install react-native-screens react-native-safe-area-context

想要使用什么导航就安装什么导航

npm i @react-navigation/native-stack
npm i @react-navigation/bottom-tabs
npm i @react-navigation/drawer
npm i @react-navigation/material-top-tabs react-native-tab-view react-native-pager-view


2. 使用

2.1 router.js 创建路由

import { createNativeStackNavigator } from '@react-navigation/native-stack'
import HomePage from ''
import AboutPage from ''

export default function Router() {
    const Stack = createNaviteStackNavigator()
    
    return (
      <Stack.Navigator
        // 所有页面设置统一样式
        screenOptions={{
          headerStyle: {}         
        }}
      >
        <Stack.Screen 
            name='Home' 
            component={ HomePage }
            options={{ 
              title: '首页',
              headerStyle: {  // 头部的样式
                  backgroundColor: ''
              },
              headerTintColor: '', // 标题颜色
            }}   // 一些配置
        ></Stack.Screen>
        <Stack.Screen 
            name='About' 
            component={ AboutPage }
            options={ ({ route }) => { title: route.params.title } }   // 可以是一个函数，返回的结果还是一个对象
        ></Stack.Screen>   
      </Stack.Navigator>
    )
}

2.2 APP.js 使用路由

import { NavigationContainer } from '@react-navigation/native'
import Router from ''


export default function App() {
    return (
      <NavigationContainer>
        <Router></Router>  
      </NavigationContainer>
    )
}

2.3 Home.js

/**
* route: 可以获取传递的参数
**/
export default function Home({ navigation, route }) {
    const handleGo = () => {
        navigation.navigate('About', {
            id: 1
        }) // 传递参数
    }
    
    return (
      <View>
        <Button onPress={ handleGo }>跳转</Button>  
      </View>
    )
}


3. 嵌套路由

import { createBottomTabNavigator } from '@react-navigation/bottom-tabs'

const Tab = createBottomTabNavigator()

export default function App() {
    return (
      <Tab.Navigator>
        <Tab.Screen name='Home' component={ HomePage }></Tab.Screen>
        <tab.Screen name='' component={} ></tab.Screen>
      </Tab.Navigator>
    )
}
```


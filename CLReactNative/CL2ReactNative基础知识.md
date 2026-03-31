# ReactNative基础知识

## 1. 样式的创建

```react
import { StyleSheet } from 'react-native'

<view style={ style.contain }></view>

const style = StyleSheet.create({
    contain{
       width: 100px
    }
})
```



## 2. 标签的使用

```jsx
1. Image标签

2. TextInput

3. Button

4. ScrollView

5. 长列表
  FlatList: 更适用于长列表数据，元素个数可以增删，和ScrollView不同的是，FlatList并不会立即渲染所有元素，而是优先渲染屏幕上可见的,  有两个属性data：数据来源  renderItem: 要渲染的数据
  
  SectionList: 如果想要渲染一组需要分组的数据，也许还带有分组标签的，就可以使用SectionList
  
6. Pressable: button的升级款

7. ActivityIndicator: 这个组件通常用于发送请求时的loading效果，转圈圈

function App(){
    return (
        <Image source=""></Image>
        
        
        <TextInput></TextInput>
        
        <Button title="按钮名称"  onPress={}></Button>
        
        <FlatList 
            data={} 
            renderItem={ () => { return ... } }
            keyExtractor={ (item, index) => index }
            onRefresh={ 下拉刷新 }
            refreshing={ 刷新的状态  布尔值 }
            onEndReached={ 上拉加载 }
        ></FlatList>
        
        <SectionList 
            data={ [{ title: '', data: [] }, { title: '', data: [] }] }
            renderItem={}
            renderSectionHeader={}
            keyExtractor={ (item, index) => index }
        ></sectionList>
        
        
        <ActivityIndicator size='large' color='#434' ></ActivityIndicator>
    )
}
```



## 3. 网络请求

使用fetch API或者axios,   要求你尽量使用https开头的请求



## 4. 工具库

```react
1. expo-image-picker   选择图片

import * as ImagePicker from 'expo-image-picker'

// 第一步需要获取权限
const permission = await ImagePicker.requestMediaLibraryPermissionsAsync()

// 第二步让用户选择图片
const pickerResult = await ImagePicker.launchImageLibraryAsync()


2. expo-sharing    分享图片

import * as Sharing from 'expo-sharing'

await Sharing.shareAsync(path)


3. @expo/vector-icons   图标库
```



## 5. ui库

1. NativeBase

   expo init 项目名称 --template @native-base/expo-template     创建全新的项目

   

   如果项目已经存在了

   npm i native-base  

   expo install react-native-svg

   expo install react-native-safe-area-context



## 6. 常用API

```react
# 1. 屏幕API

1. Dimensions 获取屏幕宽高

import { Dimensions } from 'react-native'

const { width, height } = Dimensions.get('window')


# 2. 设备API

1. Platform 主要用于获取设备的相关信息
```


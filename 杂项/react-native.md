# React Native

# 环境

- java jdk1.8以上

- Android SDK

- nodejs

  **环境搭建及连接模拟器:[https://www.cnblogs.com/jcxfighting/p/11214458.html](https://www.cnblogs.com/jcxfighting/p/11214458.html)**

## 概述

`react-native` 的环境搭建相比较于普通的web项目(vue、react）来说要繁琐不少，但是通过以下的方式基本都可以得到解决。

- 老师的文档
- [RN的官网文档](https://reactnative.cn/docs/getting-started.html)
- 百度+谷歌

## 安装环境介绍

- 操作系统：win10专业版
- 手机：`安卓手机真机`一部或夜神模拟器
- 必须安装的依赖有:Node、JDK、Yarn、Android SDK、Python2



## Node的安装

- 先到 [官网 ](http://nodejs.cn/)去下载node版本(使用 [nvm](https://github.com/coreybutler/nvm-windows) 工具来安装也可以)
- 老师当前是用的 **12.16.3** 版本
- 以 **管理员** 身份安装 然后一直点击下一步即可



## [Yarn](https://yarn.bootcss.com)的安装

Yarn是Facebook提供的替代npm的工具，可以加速node模块的下载

```js
npm install yarn -g  // 使用npm全局安装yarn 
```

检查是否安装成功

```js
yarn -v
```

**效果如下:**

![image-20200522155320956](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20200522155320956.png)



## JDK的安装与配置

> Java SE Development Kit

安卓系统的APP离不开JAVA环境，因此需要下载安装JDK(1.8版本)。到[该网站](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载JDK

![image-20200522155509416](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20200522155509416.png)

需要注意的是单击下载之后，会跳转到一个Oracler的登陆页面，得登陆之后才可以下载，如果没有账号可以注册一
个，也是比较简单，下载完成之后，以管理身份进行l默认安装。

### JDK的环境变量配置

## Android SDK的下载与安装

我们可以直接下载Android SDK并进行必要配置 
1.首先打开 [网站](https://www.androiddevtools.cn/)，然后一直向下拉，找到 SDK Tools 进行下载

![image-20200522161947215](C:/Users/Administrator/Desktop/medias/image-20200522161947215.png)

2.以管理员身份安装此软件，设置 允许使用此计算机的所有人 选项，其它一路默认到底，直到安装完成.
切记，切记，切记，重要的事情说三遍，一定记着Android SDK的安装路径，后面会打开这个管理器下载东
西

![image-20200522162022143](C:/Users/Administrator/Desktop/medias/image-20200522162022143.png)

## Android SDK的下载项

1.根据RN中文网的描述，编译React Native应用需要的是Android 9版本的SDK，还需要各种组件，为
了当前以及后期的稳定，总结起来，总共需要下载:

- Android SDK Tools 25.2.5
- Android SDK Platform-tools 29.0.5
- Android SDK Build-tools 29
- Android SDK Build-tools 28.0.3
- Android SDK Build-tools 28.0.2
- Android SDK Build-tools 28.0.1
- Android SDK Build-tools 28
- Android SDK Build-tools 27
- SDK Platform 29
- Intel x86 Atom System Image 29
- SDK Platform 28
- Intel x86 Atom System Image 28
- SDK Platform 27

2.接下来，打开`SDK Manager.exe`，照此截图依次下载以上SDK及组件

![image-20200522162144179](C:/Users/Administrator/Desktop/medias/image-20200522162144179.png)

---



![image-20200522162154907](C:/Users/Administrator/Desktop/medias/image-20200522162154907.png)

---

![image-20200522162228056](C:/Users/Administrator/Desktop/medias/image-20200522162228056.png)

---

![image-20200522162257510](C:/Users/Administrator/Desktop/medias/image-20200522162257510.png)

3.此下载的过程取决于自家的网速了，不过一般都会成功的，耐心等待安装完成即可。

## Android环境变量设置

1.右键选中 此电脑 点属性，再点高级，再点环境变量，设置如下

![image-20200522162417205](C:/Users/Administrator/Desktop/medias/image-20200522162417205.png)

---

![image-20200522162431658](C:/Users/Administrator/Desktop/medias/image-20200522162431658.png)

2.到此，安卓的环境变量配置完成

## 初始化项目和打包APP到手机

1. 准备一台  Android 手机, 通过数据线 连接 到电脑，设置启用 USB调试

2. 如果没有安卓手机，可以使用安卓模拟器也可以，推荐使用 夜神模拟器 ，自行百度下载安装

3. 一般的手机在 设置 中可以直接找到 开发者选项 进行开启, 如果 找不到 , 就自行百度查一下

   ![image-20200522170648131](C:/Users/Administrator/Desktop/medias/image-20200522170648131.png)

4. 手机连接电脑成功后运行检测命令  `adb devices` , 如果有输出设备列表与  ID 相关的字符串就证明
   手机和电脑是连接成功了，如果没有显示设备号，则说明连接有问题，一定要保证手机和电脑是正常连接状态

   ![image-20200522170726891](C:/Users/Administrator/Desktop/medias/image-20200522170726891.png)

5. 运行  `npx react-native init 项目名称` 命令初始化一个  React-Native 项目, 创建时需要联网 下载 依赖
   包, 可能比较慢，取决于各自的网速，一定要耐心等待，如果出错了，则重新运行命令再次初始化即可，例
   如：

   ```js
   npx react-native init myApp 
   ```

6. 使用 `cd myApp` 命令进行此项目文件夹，确保手机和电脑连接正常的情况下，然后再输入命令 ``adb
   devices `来检测一下手机是否正常连接，然后再使用命令 `yarn android`   将APP打包到手
   机上

7. 手机上出现如下画面，说明打包成功

   ![image-20200522170849831](C:/Users/Administrator/Desktop/medias/image-20200522170849831.png)

## 手机屏幕投影工具

为了在电脑上看到真实的手机屏幕,可以安装手机屏幕投影工具

网上有很多工具,百度即可看到。老师使用的是  [scrcpy](https://github.com/Genymobile/scrcpy)

### 使用教程

1. 下载好 工具 

2. 手机通过usb连接到电脑

3. 双击打开工具即可

   ![image-20200522172425782](C:/Users/Administrator/Desktop/medias/image-20200522172425782.png)

# 初始化项目

1. 安装SDK: [https://dl.google.com/android/installer_r24.4.1-windows.exe?utm_source=androiddevtools&utm_medium=website](https://dl.google.com/android/installer_r24.4.1-windows.exe?utm_source=androiddevtools&utm_medium=website)

![image-20210717181131629](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210717181131629.png)

2. 环境搭建完成后将手机与电脑连接在命令行窗口中使用命令检查电脑与手机是否连接[此时需要打开手机的开发者模式]`adb devices`该命令是安装sdk后(我的sdk安装在D盘的根目录下)在``D:\Android_sdk\platform-tools`	中的一个可执行程序,需要将该路径加入到系统变量`PATH`中

3. `npx react-native run-android`进行安装react-native并初始化项目安装到手机中

**将数据线连接手机并打开开发者模式允许USB调试**

# 运行项目

​	**模拟器: 例如夜神`　adb connect 127.0.0.1:62001 `**

​	`react-native run-android`

# Flex布局

	1. 默认布局为`flex`
	2. 布局方向`flex-direction: column`

# 样式

​	1. 直接在标签中使用样式

```js
<View style={{backgroundColor:"red",flex:1}}></View>
```

​	2. *样式没有继承*:`背景颜色 字体颜色 字体大小`

​	3. 不能使用`px vh vw`作为单位但可以使用百分比或纯数字作为单位,系统会自动根据屏幕的宽高进行调整安卓单位为`dp`

```js
<View style={{fontSize: 15}}></View>
```

```js
//工具类帮助解决宽度问题
import {  Dimensions} from "react-native";

//  设计稿的宽度 / 元素的宽度 = 手机屏幕 / 手机中元素的宽度
// 手机中元素的宽度 = 手机屏幕 *  元素的宽度 / 设计稿的宽度 375

/**
 * 屏幕的宽度
 */
export const screenWidth=Dimensions.get("window").width;
/**
 * 屏幕的高度
 */
export const screenHeight=Dimensions.get("window").height;

/**
 * 将px转为dp
 * @param {Number} elePx 元素的宽度或者高度 单位 px
 */
export const pxToDp=(elePx)=>screenWidth * elePx / 375;
```

**[获取屏幕宽高](https://www.react-native.cn/docs/next/dimensions)**

​	4. 文本需要使用标签`Text`包裹否则报错

	5. 变换: 需要使用数组

```js
<View style={{transform: [{translateX: 1}, {scale: 1}]}}>
     <Text>transfrom</Text>
</View>
```

# 常用标签

**[全部组件](https://www.react-native.cn/docs/next/components-and-apis)**

`View`: 类似网页中的div标签,不支持设置字体大小字体颜色等,不能直接方法文本内容,不支持直接绑定点击事件使用`TouchableOpacity`来代替

`Text`: 文本标签,支持设置字体大小和字体颜色支持绑定事件

`TouchableOpacity`: 块级容器,支持点击事件`onPress` 支持设置点击透明度`activeOpacity`

```js
const handleOnPress = () => {
  alert('handleOnPress');
}
const App = () => {
  return (
      <TouchableOpacity activeOpacity={0.5} onPress={handleOnPress}>
        <Text>TouchableOpacity</Text>
      </TouchableOpacity>
  );
};
```

`Image`: 图片标签`source`属性指定资源路径

```js
//本地资源路径
<Image source={require("./demo.png")} />
//网络资源路径,使用是必须加高度和宽度
<Image source={{url: "https://demo.com/demo.png"}} style={{height:200,width:300}}/>
```

![image-20210718142225480](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210718142225480.png)

`ImageBackground`: 背景图片,块级容器,相当于div+background,默宽高由内容大小决定,必须有宽度

```js
  <ImageBackground
        style={{ height: 200, width: 300 }}
        source={{
          url: 'https://pubic-1300732204.cos.ap-chengdu.myqcloud.com/4.jpg',
        }}
      />
```

`InputText`: 文本输入框,`onchangeText`事件获得输入框值

```js
const handleOnChange= (text)=>{
  alert(text)
}
 <TextInput onChangeText={handleOnChange}></TextInput>
```

`StatusBar`: 属性`translucent`应用会延伸到状态栏之下绘制（即所谓“沉浸式”——被状态栏遮住一部分）。常和带有半透明背景色的状态栏搭配使用`<StatusBar translucent={true} backgroundColor="transparent"/>`。

# 语法

## 插值表达式

```js
const name = "jack"
<Text>{name}</Text>
```

**{}中是js的代码块**

## 组件

​	函数式组件和类式组件与react相同,类式组件继承自React.Component

```js
//类式组件需要继承React.Component
```

## 事件



## 生命周期

​	参照react

## 调试

### 使用RN推荐方式

安装依赖: `npm i axios --save` `npm i `

修改入口文件`index.js'`

```js
import { AppRegistry } from 'react-native';
import App from './App';
import { name as appName } from './app.json';
//加入如下代码
// eslint-disable-next-line no-undef
GLOBAL.XMLHttpRequest = GLOBAL.originalXMLHttpRequest || [GLOBAL.XMLHttpRequest];
AppRegistry.registerComponent(appName, () => App);
```

下载`rn-debugger-windows-x64.zip`:[https://github.com/jhen0409/react-native-debugger/releases](https://github.com/jhen0409/react-native-debugger/releases)

或直接下载压缩包[https://github.com/jhen0409/react-native-debugger/releases/download/v0.11.9/rn-debugger-windows-x64.zip](https://github.com/jhen0409/react-native-debugger/releases/download/v0.11.9/rn-debugger-windows-x64.zip)

运行解压后文件夹里的可执行程序`react-native-debugger.exe`重新打开debug即可连接成功

## 连接夜神模拟器

　`adb connect 127.0.0.1:62001 `

## 关闭调试器警告

`打开index.js入口文件加入`

```js
console.ignoredYellowBox = ['Warning: BackAndroid is deprecated. Please use BackHandler instead.','source.uri should not be an empty string','Invalid props.style key'];
 
console.disableYellowBox = true // 关闭全部黄色警告
```

# 全局状态管理MOBX

![image-20210718175027947](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210718175027947.png)

1. ` npm i --save mobx mobx-react @babel/plugin-proposal-decorators` 安装依赖
2. 修改`babel.config.js`

```js
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [['@babel/plugin-proposal-decorators', {legacy: true}]],
};
```

3. 创建文件: 使用了es7的装饰器语法,name就是需要管理的状态

```js
import {observable, action} from 'mobx';
class rootStore {
  @observable
  myName = 'jackson';
  @action
  changeMyName =(name)=>{
    this.myName=name
  }
}
export default new rootStore();
```

![image-20210718180946553](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210718180946553.png)

4. 在`App.js`组件中挂载

```js
import React, {Component} from 'react';
import {View} from 'react-native';
import {Provider} from 'mobx-react';
import rootStore from './mobx';

class App extends Component {
  render = () => (
  <Provider rootStore={rootStore}>
      <!--其他组件-->
    <View>
    </View>
  </Provider>
  );
};

export default App;
```

![image-20210718182410171](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210718182410171.png)

5. 组件想要使用则单独取出

```js
//组件Son/index.js
import React, { Component } from 'react'
import {View,Text} from 'react-native';
import {inject,observer} from 'mobx-react';
@inject('rootStore')
@observer
export default class index extends Component {
    changeName=()=>{//修改数据
        this.props.rootStore.changeMyName("julyye")
    }
    render=()=> {
        return (
            <View>
                <Text onPress={this.changeName}>{this.props.rootStore.myName}</Text>
            </View>
        )
    }
}
//App.js
import React, {Component} from 'react';
import {View} from 'react-native';
import {Provider} from 'mobx-react';
import rootStore from './mobx';
import Son from './components/Son'
class App extends Component {
  render = () => (
  <Provider rootStore={rootStore}>
    <View>
        <Son />
    </View>
  </Provider>
  );
};

export default App;
```

# 常用库

# [react-navigation](https://www.npmjs.com/package/react-navigation)

> 页面跳转和转场动画

1. 安装

   ```js
   yarn add react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view  @react-navigation/stack @react-navigation/native
   ```

2. 代码

   ```react
   import * as React from 'react';
   import { Button, View, Text } from 'react-native';
   import { NavigationContainer } from '@react-navigation/native';
   import { createStackNavigator } from '@react-navigation/stack';
   
   function HomeScreen({ navigation }) {
     return (
       <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
         <Text>Home Screen</Text>
         <Button
           title="Go to Details"
           onPress={() => navigation.navigate('Details')}
         />
       </View>
     );
   }
   
   function DetailsScreen() {
     return (
       <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
         <Text>Details Screen</Text>
       </View>
     );
   }
   
   const Stack = createStackNavigator();
   
   function App() {
     return (
       <NavigationContainer>
         <Stack.Navigator initialRouteName="Home">
           <Stack.Screen name="Home" component={HomeScreen} />
           <Stack.Screen name="Details" component={DetailsScreen} />
         </Stack.Navigator>
       </NavigationContainer>
     );
   }
   
   export default App;
   
   ```

## 其他

1. 跳转页面

   [类组件](https://reactnavigation.org/docs/navigation-context)

   ```jsx
   import { NavigationContext } from '@react-navigation/native';
   
   class SomeComponent extends React.Component {
     static contextType = NavigationContext;
   
     render() {
       // We can access navigation object via context
       const navigation = this.context;
     }
   }
   ```

   函数组件

   ```jsx
   import * as React from 'react';
   import { Button } from 'react-native';
   import { useNavigation } from '@react-navigation/native';
   function MyBackButton() {
     const navigation = useNavigation();
     return (
       <Button
         title="Back"
         onPress={() => {
           navigation.goBack();
         }}
       />
     );
   }
   ```

# [react-native-svg-uri](https://www.npmjs.com/package/react-native-svg-uri)

> rn中使用svg技术

1. 下载

   ```js
   yarn  add  react-native-svg-uri react-native-svg
   ```

2. 代码

   ```jsx
   import React, { Component } from 'react';
   import { View, Text } from 'react-native';
   import SvgUri from 'react-native-svg-uri';
   class Index extends Component {
     render() {
       return (
         <View>
           <SvgUri width="23" height="23" svgXmlData={'<svg t="1568188030646"  viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="7085" width="64" height="64"><path d="M515.3 597.2c-72.6 0-140.8-28.3-192.2-79.6-51.3-51.3-79.6-119.6-79.6-192.2s28.3-140.8 79.6-192.2 119.6-79.6 192.2-79.6 140.8 28.3 192.2 79.6 79.6 119.6 79.6 192.2-28.3 140.8-79.6 192.2c-51.4 51.3-119.6 79.6-192.2 79.6z m0-503.5c-61.9 0-120.1 24.1-163.9 67.9s-67.9 102-67.9 163.9 24.1 120.1 67.9 163.9c43.8 43.8 102 67.9 163.9 67.9 61.9 0 120.1-24.1 163.9-67.9 43.8-43.8 67.9-102 67.9-163.9 0-61.9-24.1-120.1-67.9-163.9-43.8-43.8-102-67.9-163.9-67.9zM994.9 915.6H62.3l3.8-23.3c12.4-75 51.3-143.9 109.6-194 59-50.6 134-78.5 211.3-78.5h283.1c77.3 0 152.3 27.9 211.3 78.5 58.3 50 97.2 118.9 109.6 194l3.9 23.3z m-884.4-40h836.2c-14.4-56.7-46.2-108.2-91.3-146.9-51.7-44.3-117.5-68.7-185.2-68.7H387c-67.7 0-133.5 24.4-185.2 68.8-45.1 38.7-77 90.2-91.3 146.8z" p-id="7086" fill="#999999"></path><path d="M448.1 640L513 841l80-201zM437.7 345c-12.1 0-21.9-9.9-21.9-21.9V269c0-12.1 9.9-21.9 21.9-21.9 12.1 0 21.9 9.9 21.9 21.9v54.1c0.1 12-9.8 21.9-21.9 21.9zM573.2 345c-12.1 0-21.9-9.9-21.9-21.9V269c0-12.1 9.9-21.9 21.9-21.9 12.1 0 21.9 9.9 21.9 21.9v54.1c0.1 12-9.8 21.9-21.9 21.9z" p-id="7087" fill="#999999" ></path></svg>'} />
         </View>
       );
     }
   }
   export default Index;
   ```

   

# [react-native-tab-navigator](https://www.npmjs.com/package/react-native-tab-navigator)

> 底部导航栏

1. 下载

   ```js
   yarn add react-native-tab-navigator
   ```

2. 代码

   要有 SvgUri 和 Friend 等其他依赖

   ```jsx
   import React, { Component } from 'react';
   import { View, Text,StyleSheet } from 'react-native';
   import SvgUri from 'react-native-svg-uri';
   import Friend from "./pages/friend";
   import Group from "./pages/group";
   import Message from "./pages/message";
   import My from "./pages/my";
   import TabNavigator from 'react-native-tab-navigator';
   import SvgData from "./res/svg";
   const dataSource = [
     {
       icon: SvgData.friend,
       selectedIcon: SvgData.selectdFriend,
       tabPage: 'Friend',
       tabName: '交友',
       badge: 0,
       component: Friend
     },
     {
       icon: SvgData.group,
       selectedIcon:  SvgData.selectdGroup,
       tabPage: 'Group',
       tabName: '圈子',
       badge: 0,
       component: Group
     },
     {
       icon: SvgData.message,
       selectedIcon:SvgData.selectdMessage,
       tabPage: 'Message',
       tabName: '消息',
       badge: 5,
       component: Message
     },
     {
       icon: SvgData.my,
       selectedIcon: SvgData.selectdMy,
       tabPage: 'My',
       tabName: '我的',
       badge: 0,
       component: My
     }
   
   ];
   
   class Index extends Component {
     state = {
       selectedTab: "Friend"
     }
     render() {
       return (
         <View style={{ flex: 1, backgroundColor: '#F5FCFF' }}>
           <TabNavigator   >
             {dataSource.map((v, i) => {
               return (
                 <TabNavigator.Item
                   key={i}
                   selected={this.state.selectedTab === v.tabPage}
                   title={v.tabName}
                   tabStyle={stylesheet.tab}
                   titleStyle={{ color: '#999999' }}
                   selectedTitleStyle={{ color: '#c863b5' }}
                   renderIcon={() => <SvgUri width="23" height="23" svgXmlData={v.icon} />}
                   renderSelectedIcon={() => <SvgUri width="23" height="23" svgXmlData={v.selectedIcon} />}
                   badgeText={v.badge}
                   onPress={() => this.setState({ selectedTab: v.tabPage })}>
                   <v.component  />
                 </TabNavigator.Item>
               )
             })}
           </TabNavigator>
         </View>
       )
     }
   }
   const stylesheet = StyleSheet.create({
     tab: {
       justifyContent: "center"
     },
     tabIcon: {
       color: "#999",
       width: 23,
       height: 23
     }
   })
   export default Index;
   ```





# [react-native-element](https://react-native-elements.github.io/react-native-elements/docs/getting_started.html)

> 一套ui库 内置常用组件

1. 下载

   需要使用到图标 因此也需要安装 `react-native-vector-icons`

   ```js
   yarn add react-native-elements react-native-vector-icons
   ```

2. 引入和使用

   ```jsx
   import { Icon } from 'react-native-elements'
   
   <Icon
     name='rowing' />
   ```

3. [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons) 的其他使用

   1. 编辑 `android/app/build.gradle` 

   2. 添加以下配置

      ```jsx
      project.ext.vectoricons = [
          iconFontNames: [ 'MaterialIcons.ttf', 'EvilIcons.ttf' ] // Name of the font files you want to copy
      ]
      
      apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
      ```

   3. 重启项目

   4. 添加代码 如

      ```jsx
      import FontAwesome5 from 'react-native-vector-icons/FontAwesome5';
      
      const icon = <FontAwesome5 name={'comments'} />;
      ```





# [react-native-linear-gradient](https://www.npmjs.com/package/react-native-linear-gradient)

> 渐变容器

1. 下载

   ```js
   yarn add react-native-linear-gradient
   ```

2. 简单使用

   ```jsx
   import LinearGradient from 'react-native-linear-gradient';
    
   <LinearGradient colors={['#4c669f', '#3b5998', '#192f6a']} style={styles.linearGradient}>
     <Text style={styles.buttonText}>
       Sign in with Facebook
     </Text>
   </LinearGradient>
   
   var styles = StyleSheet.create({
     linearGradient: {
       flex: 1,
       paddingLeft: 15,
       paddingRight: 15,
       borderRadius: 5
     },
     buttonText: {
       fontSize: 18,
       fontFamily: 'Gill Sans',
       textAlign: 'center',
       margin: 10,
       color: '#ffffff',
       backgroundColor: 'transparent',
     },
   });
   ```





# [react-native-confirmation-code-field](https://www.npmjs.com/package/react-native-confirmation-code-field)

> 验证码输入框

1. 下载

   ```js
   yarn add react-native-confirmation-code-field
   ```

2. 代码

   ```jsx
   import React, {useState} from 'react';
   import {SafeAreaView, Text, StyleSheet} from 'react-native';
    
   import {
     CodeField,
     Cursor,
     useBlurOnFulfill,
     useClearByFocusCell,
   } from 'react-native-confirmation-code-field';
    
   const styles = StyleSheet.create({
     root: {flex: 1, padding: 20},
     title: {textAlign: 'center', fontSize: 30},
     codeFiledRoot: {marginTop: 20},
     cell: {
       width: 40,
       height: 40,
       lineHeight: 38,
       fontSize: 24,
       borderWidth: 2,
       borderColor: '#00000030',
       textAlign: 'center',
     },
     focusCell: {
       borderColor: '#000',
     },
   });
    
   const CELL_COUNT = 6;
    
   const App = () => {
     const [value, setValue] = useState('');
     const ref = useBlurOnFulfill({value, cellCount: CELL_COUNT});
     const [props, getCellOnLayoutHandler] = useClearByFocusCell({
       value,
       setValue,
     });
    
     return (
       <SafeAreaView style={styles.root}>
         <Text style={styles.title}>Verification</Text>
         <CodeField
           ref={ref}
           {...props}
           value={value}
           onChangeText={setValue}
           cellCount={CELL_COUNT}
           rootStyle={styles.codeFiledRoot}
           keyboardType="number-pad"
           textContentType="oneTimeCode"
           renderCell={({index, symbol, isFocused}) => (
             <Text
               key={index}
               style={[styles.cell, isFocused && styles.focusCell]}
               onLayout={getCellOnLayoutHandler(index)}>
               {symbol || (isFocused ? <Cursor /> : null)}
             </Text>
           )}
         />
       </SafeAreaView>
     );
   };
    
   export default App;
   ```



# [axios](https://github.com/axios/axios)

> 流行的异步请求库

1. 下载

   ```js
   yarn add axios
   ```



# [Teaset](https://github.com/rilyu/teaset/blob/HEAD/docs/cn/README.md)

> React Native UI 组件库, 超过 20 个纯 JS(ES6) 组件, 专注于内容展示和操作控制

1. 下载

   ```js
   yarn add teaset
   ```










# svg

> 因为在rn中 使用普通的字体图标是没有多种颜色的如

![image-20200605103850857](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20200605103850857.png)

1. 下载依赖

   ```js
   yarn add react-native-svg react-native-svg-uri
   ```

2. 复制 示例demo中svg源代码

   ![image-20200605104237256](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20200605104237256.png)

3.  代码中使用

   ```jsx
   import SvgUri from 'react-native-svg-uri';
   const female='<svg ......';
   
    <SvgUri width="23" height="23" svgXmlData={female} />
   ```

   

   

   

# iconfont字体图标

1. 在字体图标网站上下载 字体 

2. 然后拷贝 ttf后缀的文件到 `android\app\src\main\assets\fonts`中  如果没有`assets`文件夹可以新建一个

3. 然后 给 `Text` 标签 设置

   ![image-20200605105305029](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20200605105305029.png)

   ```jsx
    <Text style={{ fontFamily: "iconfont", color: "red" }} >{'\ue82b'}</Text>
   ```
   
4. 然后记得重启项目





#  [react-native-datepicker](https://www.npmjs.com/package/react-native-datepicker)

> 日期选择框

1. 安装

   ```js
   yarn add  react-native-datepicker 
   ```

2. 引入

   ```js
   import DatePicker from 'react-native-datepicker';
   ```

3. 使用

   ```react
    <DatePicker
                 style={{ width: Styleskits.screen.width - 50 }} 
                 mode="date"
                 placeholder="设置生日"
                 format="YYYY-MM-DD"
                 confirmBtnText="Confirm"
                 cancelBtnText="Cancel"
                 iconComponent={<Icon name="angle-down"   />}
                 androidMode="spinner"
                 customStyles={{
                   dateInput: {
                     borderWidth: 0,
                     borderBottomWidth: 1.1,
                     alignItems:"flex-start",
                     paddingLeft:6,
                     textAlign:"left"
                   },
                   placeholderText:{
                     fontSize:18,
                     color:"#afafaf"
                   }
                 }}
                 onDateChange={(date) => { this.setState({ birthday: date }) }}
        />
   ```




# 使用 [react-native-amap-geolocation](https://github.com/qiuxiang/react-native-amap-geolocation)

> 高德地图组件
>
> 分别使用了两个功能，一个是AndroidSDK和一个web服务

1. [申请 高度地图的key](https://lbs.amap.com/api/android-location-sdk/guide/create-project/get-key)

2. 下载依赖

   ```js
   yarn add  react-native-amap-geolocation
   ```

7. 配置文件

   1. 编辑 `android/settings.gradle`，设置项目路径：
   
      ```diff
      + include ':react-native-amap-geolocation'
      + project(':react-native-amap-geolocation').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-amap-geolocation/lib/android')
      ```
   
   2. 编辑 `android/app/build.gradle`，新增依赖：
   
      ```diff
      dependencies {
      +   implementation project(':react-native-amap-geolocation')
      }
      ```
   
   3. 编辑 `MainApplication.java`：
   
      ```diff
      + import cn.qiuxiang.react.geolocation.AMapGeolocationPackage;
      
      public class MainApplication extends Application implements ReactApplication {
        @Override
              protected List<ReactPackage> getPackages() {
                @SuppressWarnings("UnnecessaryLocalVariable")
                List<ReactPackage> packages = new PackageList(this).getPackages();
                // Packages that cannot be autolinked yet can be added manually here, for example:
      +         packages.add(new AMapGeolocationPackage());
                return packages;
              }
      }
      ```
   
4. 代码

   ```js
   
   import { PermissionsAndroid, Platform } from "react-native";
   import { init, Geolocation } from "react-native-amap-geolocation";
   import axios from "axios";
   class Geo {
     async initGeo() {
       if (Platform.OS === "android") {
         await PermissionsAndroid.request(PermissionsAndroid.PERMISSIONS.ACCESS_COARSE_LOCATION);
       }
       await init({
         ios: "e8b092f4b23cef186bd1c4fdd975bf38",
         android: "e8b092f4b23cef186bd1c4fdd975bf38"
       });
       return Promise.resolve();
     }
     async getCurrentPosition() {
       return new Promise((resolve, reject) => {
         console.log("开始定位");
         Geolocation.getCurrentPosition(({ coords }) => {
           resolve(coords);
         }, reject);
       })
     }
     async getCityByLocation() {
       const { longitude, latitude } = await this.getCurrentPosition();
       const res = await axios.get("https://restapi.amap.com/v3/geocode/regeo", {
         params: { location: `${longitude},${latitude}`, key: "83e9dd6dfc3ad5925fc228c14eb3b4d6", }
       });
       return Promise.resolve(res.data);
     }
   }
   
   
   export default new Geo();
   ```



#  [react-native-picker](https://www.npmjs.com/package/react-native-picker)

> 自定义picker

1. 安装

   ```
   yarn add react-native-picker
   ```

2. 代码

   ```react
   import Picker from 'react-native-picker';
       Picker.init({
         pickerData: CityJson,
         selectedValue: ["北京", "北京"],
         wheelFlex: [1, 1, 0], // 显示省和市
         pickerConfirmBtnText: "确定",
         pickerCancelBtnText: "取消",
         pickerTitleText: "选择城市",
         onPickerConfirm: data => {
           // data =  [广东，广州，天河]
           this.setState(
             {
               city: data[1]
             }
           );
         }
       });
       Picker.show();
   ```
   
   
   
   







# 使用 [react-native-image-crop-picker](https://www.npmjs.com/package/react-native-image-crop-picker)

> 图片裁切组件

1. 安装

   ```
   yarn add  react-native-image-crop-picker 
   ```

2. 使用

   ```react
   import ImagePicker from 'react-native-image-crop-picker';
   
       ImagePicker.openPicker({
         width: 300,
         height: 400,
         cropping: true
       }).then(image => {
         console.log(image);
       });
   ```



# [jmessage-react-plugin](https://github.com/jpush/jmessage-react-plugin)

> 极光推送 react-native 版本

## [开通服务](https://www.jiguang.cn)

1. 首先 需要我们自己先在极光上注册账号 开通服务,拿到对应的密钥

## 简单使用

1. 安装依赖

   ```js
   yarn add jmessage-react-plugin jcore-react-native
   ```

2. 配置

   1. `android\app\src\main\AndroidManifest.xml` 加入以下代码

      ```xml
            <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
            <!-- 极光的配置 -->
            <meta-data android:name="JPUSH_CHANNEL" android:value="${APP_CHANNEL}" />
            <meta-data android:name="JPUSH_APPKEY" android:value="${JPUSH_APPKEY}" />
            <!-- 极光的配置 -->
          </application>
      ```

   2. `android\app\build.gradle` 加入以下代码和按需修改

      ```js
      android {
          compileSdkVersion rootProject.ext.compileSdkVersion
      
          compileOptions {
              sourceCompatibility JavaVersion.VERSION_1_8
              targetCompatibility JavaVersion.VERSION_1_8
          }
      
          defaultConfig {
              applicationId "com.awesomeproject22"
              minSdkVersion rootProject.ext.minSdkVersion
              targetSdkVersion rootProject.ext.targetSdkVersion
              versionCode 1
              versionName "1.0"
              multiDexEnabled true // 新增的
              manifestPlaceholders = [
              JPUSH_APPKEY: "c0c08d3d8babc318fe25bb0c",	//在此替换你的APPKey
              APP_CHANNEL: "developer-default"		//应用渠道号
              ]
          }
      ```

      ---

      ```js
      dependencies {
          implementation fileTree(dir: "libs", include: ["*.jar"])
          implementation "com.facebook.react:react-native:+"  // From node_modules
          compile project(':jmessage-react-plugin') // 新增的
          compile project(':jcore-react-native')  // 新增的
          if (enableHermes) {
              def hermesPath = "../../node_modules/hermes-engine/android/";
              debugImplementation files(hermesPath + "hermes-debug.aar")
              releaseImplementation files(hermesPath + "hermes-release.aar")
          } else {
              implementation jscFlavor
          }
      }
      ```

   3. 根目录下新建文件和添加以下配置 `react-native.config.js`

      ```js
      module.exports = {
        dependencies: {
          'jmessage-react-plugin': {
            platforms: {
              android: {
                packageInstance: 'new JMessageReactPackage(false)'
              }
            }
          },
        }
      };
      ```

   4.  `android\settings.gradle` 加入如下配置

      ```js
      include ':jmessage-react-plugin'
      project(':jmessage-react-plugin').projectDir = new File(rootProject.projectDir, '../node_modules/jmessage-react-plugin/android')
      include ':jcore-react-native'
      project(':jcore-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jcore-react-native/android')
      ```

3. 在 根组件中进行测试 `App.js`

   ```react
   import React from 'react';
   import { View, Text } from "react-native";
   import JMessage from "jmessage-react-plugin";
   class App extends React.Component {
     componentDidMount() {
       JMessage.init({
         'appkey': 'c0c08d3d8babc318fe25bb0c',
         'isOpenMessageRoaming': true,
         'isProduction': false,
         'channel': '' 
       })
   
       JMessage.login({
         username: "18665711956",
         password: "18665711956"
       }, (res) => {
         console.log("登录成功");
         console.log(res);
       }, (err) => {
         console.log("登录失败");
         console.log(err);
       })
   
     }
     render() {
       return (
         <View>
           <Text>goods</Text>
         </View>
       );
     }
   }
   export default App;
   ```
   
   







# [react-native-image-header-scroll-view](https://www.npmjs.com/package/react-native-image-header-scroll-view)

> 顶部吸顶效果

1. 下载

   ```js
   yarn add react-native-image-header-scroll-view 
   ```

   

# 小技巧

## 关闭黄色警告

```jsx
console.ignoredYellowBox = ['Warning: BackAndroid is deprecated. Please use BackHandler instead.','source.uri should not be an empty string','Invalid props.style key'];
 
console.disableYellowBox = true // 关闭全部黄色警告
```





# [react-native-deck-swiper](https://www.npmjs.com/package/react-native-deck-swiper)

> 类似轮播图的滑动组件

1. 下载

   ```js
   yarn add react-native-view-overflow  react-native-deck-swiper
   ```

2. 示例demo

   ```jsx
   import Swiper from "react-native-deck-swiper";
   
   render () {
       <View style={styles.container}>
           <Swiper
               cards={['DO', 'MORE', 'OF', 'WHAT', 'MAKES', 'YOU', 'HAPPY']}
               renderCard={(card) => {
                   return (
                       <View style={styles.card}>
                           <Text style={styles.text}>{card}</Text>
                       </View>
                   )
               }}
               onSwiped={(cardIndex) => {console.log(cardIndex)}}
               onSwipedAll={() => {console.log('onSwipedAll')}}
               cardIndex={0}
               backgroundColor={'#4FD0E9'}
               stackSize= {3}>
               <Button
                   onPress={() => {console.log('oulala')}}
                   title="Press me">
                   You can press me
               </Button>
           </Swiper>
       </View>
   }
   
   const styles = StyleSheet.create({
     container: {
       flex: 1,
       backgroundColor: "#F5FCFF"
     },
     card: {
       flex: 1,
       borderRadius: 4,
       borderWidth: 2,
       borderColor: "#E8E8E8",
       justifyContent: "center",
       backgroundColor: "white"
     },
     text: {
       textAlign: "center",
       fontSize: 50,
       backgroundColor: "transparent"
     }
   });
   ```


# [react-native-image-zoom-viewer](https://www.npmjs.com/package/react-native-image-zoom-viewer)  

> 图片缩放组件

1. 下载

   ```
   yarn add react-native-image-zoom-viewer
   ```

2. 代码

   ```jsx
   import { Modal } from 'react-native';
   import ImageViewer from 'react-native-image-zoom-viewer';
   
   const images = [{
     // Simplest usage.
     url: 'https://avatars2.githubusercontent.com/u/7970947?v=3&s=460',
   
     // width: number
     // height: number
     // Optional, if you know the image size, you can set the optimization performance
   
     // You can pass props to <Image />.
     props: {
       // headers: ...
     }
   }, {
     url: '',
     props: {
       // Or you can set source directory.
       source: require('../background.png')
     }
   }]
   
   export default class App extends React.Component {
     render: function() {
     return (
       <Modal visible={true} transparent={true}>
         <ImageViewer imageUrls={images} />
       </Modal>
     )
   }
   }
   ```




# [aurora-imui-react-native](https://github.com/jpush/aurora-imui/blob/master/README_zh.md)

> Aurora IMUI 是个通用的即时通讯（IM）UI 库，不特定于任何 IM SDK。
>
> 本 UI 库提供了消息列表、输入视图等常用组件，支持常见的消息类型：文字、图片、语音、视频等。默认包含多套界面风格，也能根据自己的需要自定义。



1. 安装

   ```js
   yarn  add  aurora-imui-react-native  react-native-fs
   ```

2. 配置 `引入 Package`

   > MainApplication.java

   ```js
   import cn.jiguang.imui.messagelist.ReactIMUIPackage; // 新增的
   
   @Override
           protected List<ReactPackage> getPackages() {
             @SuppressWarnings("UnnecessaryLocalVariable")
             List<ReactPackage> packages = new PackageList(this).getPackages();
              packages.add(new AMapGeolocationPackage());
              packages.add(new ReactIMUIPackage()); // 新增的
             return packages;
           }
   ```

3. `android\settings.gradle` 添加配置

   ```js
   include ':app', ':aurora-imui-react-native'
   project(':aurora-imui-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/aurora-imui-react-native/ReactNative/android')
   ```

4. `android\app\build.gradle` 添加配置

   ```js
   dependencies {
   	...
       implementation project(':aurora-imui-react-native')
   ```

5. `android\app\src\main\AndroidManifest.xml` 

   ```xml
   <manifest 
    xmlns:tools="http://schemas.android.com/tools" // 这里是新添加的
   >
       <application
         android:allowBackup="false"
         tools:replace="android:allowBackup" // 这里是新添加的
         >
   ```

6. [实例demo](https://github.com/jpush/aurora-imui/blob/master/ReactNative/sample/App.js)



# [react-native-scrollable-tab-view](https://www.npmjs.com/package/react-native-scrollable-tab-view)

> 顶部tab栏

## 使用

1. 安装依赖

   ```
   yarn add   react-native-scrollable-tab-view @react-native-community/viewpager
   ```

2. 使用

   > 标签最外层必须为 `ScrollableTabView`

   ```jsx
     
   import React from 'react';
   import {
     Text,
   } from 'react-native';
   
   import ScrollableTabView, { DefaultTabBar } from 'react-native-scrollable-tab-view';
   
   export default () => {
     return <ScrollableTabView
       style={{ marginTop: 20 }}
       initialPage={1}
       renderTabBar={() => <DefaultTabBar />}
     >
       <Text tabLabel='Tab #1'>My</Text>
       <Text tabLabel='Tab #2'>favorite</Text>
       <Text tabLabel='Tab #3'>project</Text>
     </ScrollableTabView>;
   }
   ```





# [moment](http://momentjs.cn/)

> 流行的日期库

1. 下载

   ```js
   yarn add moment   
   ```

2. 设置语言

   ```js
   import moment from "moment";
   import 'moment/locale/zh-cn';
   moment.locale('zh-cn');
   
   export default moment;
   ```



# [react-native-image-picker 图片选择工具](https://github.com/react-native-community/react-native-image-picker)

1. 安装

   ```js
   yarn add react-native-image-picker
   ```

3. 代码

   ```jsx
   import ImagePicker from 'react-native-image-picker';
   
   // More info on all the options is below in the API Reference... just some common use cases shown here
   const options = {
     title: 'Select Avatar',
     customButtons: [{ name: 'fb', title: 'Choose Photo from Facebook' }],
     storageOptions: {
       skipBackup: true,
       path: 'images',
     },
   };
   
   /**
    * The first arg is the options object for customization (it can also be null or omitted for default options),
    * The second arg is the callback which sends object: response (more info in the API Reference)
    */
   ImagePicker.showImagePicker(options, (response) => {
     console.log('Response = ', response);
   
     if (response.didCancel) {
       console.log('User cancelled image picker');
     } else if (response.error) {
       console.log('ImagePicker Error: ', response.error);
     } else if (response.customButton) {
       console.log('User tapped custom button: ', response.customButton);
     } else {
       const source = { uri: response.uri };
   
       // You can also display the image using data:
       // const source = { uri: 'data:image/jpeg;base64,' + response.data };
   
       this.setState({
         avatarSource: source,
       });
     }
   });
   ```


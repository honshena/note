# uni-app

# 环境

## HBuilderX

 [官网下载地址](https://www.dcloud.io/hbuilderx.html)	*选择开发版*

## uniapp文档

 [官网文档](https://uniapp.dcloud.io/README)

# 路由跳转

`uni-app` 有两种页面路由跳转方式：使用[navigator](https://uniapp.dcloud.io/component/navigator)组件跳转、调用[API](https://uniapp.dcloud.io/api/router)跳转。

### [页面栈](https://uniapp.dcloud.io/frame?id=页面栈)

框架以栈的形式管理当前所有页面， 当发生路由切换的时候，页面栈的表现如下：

| 路由方式   | 页面栈表现                        | 触发时机                                                     |
| ---------- | --------------------------------- | ------------------------------------------------------------ |
| 初始化     | 新页面入栈                        | uni-app 打开的第一个页面                                     |
| 打开新页面 | 新页面入栈                        | 调用 API  [uni.navigateTo](https://uniapp.dcloud.io/api/router?id=navigateto) 、使用组件  [](https://uniapp.dcloud.io/component/navigator?id=navigator) |
| 页面重定向 | 当前页面出栈，新页面入栈          | 调用 API  [uni.redirectTo](https://uniapp.dcloud.io/api/router?id=redirectto) 、使用组件 [](https://uniapp.dcloud.io/component/navigator?id=navigator) |
| 页面返回   | 页面不断出栈，直到目标返回页      | 调用 API  [uni.navigateBack](https://uniapp.dcloud.io/api/router?id=navigateback)  、使用组件 [](https://uniapp.dcloud.io/component/navigator?id=navigator) 、用户按左上角返回按钮、安卓用户点击物理back按键 |
| Tab 切换   | 页面全部出栈，只留下新的 Tab 页面 | 调用 API  [uni.switchTab](https://uniapp.dcloud.io/api/router?id=switchtab) 、使用组件 [](https://uniapp.dcloud.io/component/navigator?id=navigator) 、用户切换 Tab |
| 重加载     | 页面全部出栈，只留下新的页面      | 调用 API  [uni.reLaunch](https://uniapp.dcloud.io/api/router?id=relaunch) 、使用组件  [](https://uniapp.dcloud.io/component/navigator?id=navigator) |

# [尺寸单位](https://uniapp.dcloud.io/frame?id=尺寸单位)

`uni-app` 支持的通用 css 单位包括 px、rpx

- px 即屏幕像素
- rpx 即响应式px，一种根据屏幕宽度自适应的动态单位。以750宽的屏幕为基准，750rpx恰好为屏幕宽度。屏幕变宽，rpx 实际显示效果会等比放大，但在 App 端和 H5 端屏幕宽度达到 960px 时，默认将按照 375px 的屏幕宽度进行计算，具体配置参考：[rpx计算配置](https://uniapp.dcloud.io/collocation/pages?id=globalstyle) 。

vue页面支持下面这些普通H5单位，但在nvue里不支持：

- rem 根字体大小可以通过 [page-meta](https://uniapp.dcloud.io/component/page-meta?id=page-meta) 配置
- vh viewpoint height，视窗高度，1vh等于视窗高度的1%
- vw viewpoint width，视窗宽度，1vw等于视窗宽度的1%

nvue还不支持百分比单位。

App端，在 pages.json 里的 titleNView 或页面里写的 plus api 中涉及的单位，只支持 px。**注意此时不支持 rpx**

nvue中，uni-app 模式（[nvue 不同编译模式介绍](https://ask.dcloud.net.cn/article/36074)）可以使用 px 、rpx，表现与 vue 中一致。weex 模式目前遵循weex的单位，它的单位比较特殊：

- px:，以750宽的屏幕为基准动态计算的长度单位，与 vue 页面中的 rpx 理念相同。（一定要注意 weex 模式的 px，和 vue 里的 px 逻辑不一样。）
- wx：与设备屏幕宽度无关的长度单位，与 vue 页面中的 px 理念相同

下面对 `rpx` 详细说明：

设计师在提供设计图时，一般只提供一个分辨率的图。

严格按设计图标注的 px 做开发，在不同宽度的手机上界面很容易变形。

而且主要是宽度变形。高度一般因为有滚动条，不容易出问题。由此，引发了较强的动态宽度单位需求。

微信小程序设计了 rpx 解决这个问题。`uni-app` 在 App 端、H5 端都支持了 `rpx`，并且可以配置不同屏幕宽度的计算方式，具体参考：[rpx计算配置](https://uniapp.dcloud.io/collocation/pages?id=globalstyle)。

开发者可以通过设计稿基准宽度计算页面元素 rpx 值，设计稿 1px 与框架样式 1rpx 转换公式如下：

```
设计稿 1px / 设计稿基准宽度 = 框架样式 1rpx / 750rpx
```

换言之，页面元素宽度在 `uni-app` 中的宽度计算公式：

```
750 * 元素在设计稿中的宽度 / 设计稿基准宽度
```

#  [CSS变量](https://uniapp.dcloud.io/frame?id=css变量)

uni-app 提供内置 CSS 变量

| CSS变量             | 描述                   | App                                                          | 小程序 | H5                   |
| :------------------ | :--------------------- | :----------------------------------------------------------- | :----- | :------------------- |
| --status-bar-height | 系统状态栏高度         | [系统状态栏高度](http://www.html5plus.org/doc/zh_cn/navigator.html#plus.navigator.getStatusbarHeight)、nvue注意见下 | 25px   | 0                    |
| --window-top        | 内容区域距离顶部的距离 | 0                                                            | 0      | NavigationBar 的高度 |
| --window-bottom     | 内容区域距离底部的距离 | 0                                                            | 0      | TabBar 的高度        |

**注意：**

- `var(--status-bar-height)` 此变量在微信小程序环境为固定 `25px`，在 App 里为手机实际状态栏高度。
- 当设置 `"navigationStyle":"custom"` 取消原生导航栏后，由于窗体为沉浸式，占据了状态栏位置。此时可以使用一个高度为 `var(--status-bar-height)` 的 view 放在页面顶部，避免页面内容出现在状态栏。
- 由于在H5端，不存在原生导航栏和tabbar，也是前端div模拟。如果设置了一个固定位置的居底view，在小程序和App端是在tabbar上方，但在H5端会与tabbar重叠。此时可使用`--window-bottom`，不管在哪个端，都是固定在tabbar上方。
- 目前 nvue 在App端，还不支持 `--status-bar-height`变量，替代方案是在页面onLoad时通过uni.getSystemInfoSync().statusBarHeight获取状态栏高度，然后通过style绑定方式给占位view设定高度。下方提供了示例代码

**代码块**

快速书写css变量的方法是：在css中敲hei，在候选助手中即可看到3个css变量。（HBuilderX 1.9.6以上支持）

示例1 - 普通页面使用css变量：

```html
<template>
    <!-- HBuilderX 2.6.3+ 新增 page-meta, 详情：https://uniapp.dcloud.io/component/page-meta -->
    <page-meta>
        <navigation-bar />
    </page-meta>
    <view>
        <view class="status_bar">
            <!-- 这里是状态栏 -->
        </view>
        <view> 状态栏下的文字 </view>
    </view>
</template>    
<style>
    .status_bar {
        height: var(--status-bar-height);
        width: 100%;
    }
</style>
<template>
    <view>
        <view class="toTop">
            <!-- 这里可以放一个向上箭头，它距离底部tabbar上浮10px-->
        </view>
    </view>
</template>    
<style>
    .toTop {
        bottom: calc(var(--window-bottom) + 10px)
    }
</style>
```

示例2 - nvue页面获取状态栏高度

```html
<template>
    <view class="content">
        <view :style="{ height: iStatusBarHeight + 'px'}"></view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                iStatusBarHeight:0
            }
        },
        onLoad() {
            this.iStatusBarHeight = uni.getSystemInfoSync().statusBarHeight
        }
    }
</script>
```

### [固定值](https://uniapp.dcloud.io/frame?id=固定值)

`uni-app` 中以下组件的高度是固定的，不可修改：

| 组件          | 描述       | App                                                          | H5   |
| :------------ | :--------- | :----------------------------------------------------------- | :--- |
| NavigationBar | 导航栏     | 44px                                                         | 44px |
| TabBar        | 底部选项卡 | HBuilderX 2.3.4之前为56px，2.3.4起和H5调为一致，统一为 50px。但可以自主更改高度） | 50px |

# 语法

​	参考Vue

# Pages.json

[官网配置](https://uniapp.dcloud.io/collocation/pages?id=%e9%85%8d%e7%bd%ae%e9%a1%b9%e5%88%97%e8%a1%a8)


# Mini Program

## 环境搭建

- 注册小程序账号 365765383@qq.com
- 激活邮箱
- 信息登记
- 登陆小程序管理后台
- 完善小程序信息
- 绑定开发者

## 项目基本结构

- 项目级别
  - `app.js`
  - `app.json`
  - `app.wxss`
  - `project.config.json`
- 页面级别
  - `xx.js`
  - `xx.json`
  - `xx.wxss`
  - `xx.wxml`
- 工具类
  - `utils/`

## WXML

### 数据绑定

使用 Mustache 写法。属性/内容都适用。

```html
<view>
  <text data-name="{{theName}}">
    {{message}}
  </text>
</view>
```

```json
data: {
    message:"forza juven",
    theName:"frank"
}
```

### 列表渲染

```html
<view>
  <block wx:for="{{items}}" wx:for-item="item" wx:key="index">
    <view>{{index}}:{{item.name}}</view>
  </block>
</view>
```

```json
data: {
    items: [
      { name: "A" },
      { name: "B" },
      { name: "C" },
      { name: "D" },
      { name: "A" },
    ]
}
```

### 条件渲染

```html
<view>喜爱的食物</view>
<view wx:if="{{condition === 1}}">面</view>
<view wx:elif="{{condition === 2}}">饭</view>
<view wx:else>饺子</view>
```

```json
data: {
    condition: Math.floor(Math.random()*3+1)
}
```

#### hidden 与 if 的取舍

hidden 和 if 都能达到隐藏的效果。hidden 有很大的初始化消耗，if 有很大的切换消耗。当应用场景是频繁的隐藏或显示，优先使用 hidden 方式。

### 模板引用

```html
<!-- 模板定义 -->
<template name="tempItem">
    <view>
        <view>{{name}}</view>
        <view>{{phone}}</view>
        <view>{{address}}</view>
    </view>
</template>

<!-- 模板使用 -->
<template is="tempItem" data="{{...item}}"></template>
```

```json
data: {
    item: {
        name: "frank",
        phone: "452124151",
        address: "hangzhou"
    }
}
```

#### import

只引用另一个文件中的`tempalte定义`。

```html
<import src=""></import>
```

#### include

应用另一个文件中的除了`tempalte定义`的所有内容

```html
<include src=""></include>
```

## WXSS

### rpx

可以根据屏幕宽度进行自适应。规定屏幕宽为 750rpx。如在 iPhone6 上，屏幕宽度为 375px，共有 750 个物理像素，则 750rpx = 375px = 750 物理像素，1rpx = 0.5px = 1 物理像素。

### 样式

- 外联样式

  ```html
  @import 'xx.wxss';
  ```

- 内联样式

  ```html
  <view style="xx"></view>
  ```

## WXS

WXS（WeiXin Script）是小程序的一套脚本语言，结合 `WXML`，可以构建出页面的结构。其实就是对 JavaScript 的封装及限制。

其特点为：

1. wxs 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行。
2. wxs 与 javascript 是不同的语言，有自己的语法，并不和 javascript 一致。
3. wxs 的运行环境和其他 javascript 代码是隔离的，wxs 中不能调用其他 javascript 文件中定义的函数，也不能调用小程序提供的 API。
4. wxs 函数不能作为组件的事件回调。
5. 由于运行环境的差异，在 iOS 设备上小程序内的 wxs 会比 javascript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。

### 写法

```html
<wxs module="foo">
  var some_msg = "hello world"; module.exports = { msg : some_msg, }
</wxs>
<view>{{foo.msg}}</view>
```

```html
<wxs src="./../comm.wxs" module="some_comms"></wxs>
<view>{{some_comms.bar(some_comms.foo)}}</view>
```

## MINA 框架

![mina](C:\Users\frank.hu\Documents\notes\MiniProgram\mina.png)

WXML 模板和 WXSS 样式工作在渲染层，JS 脚本工作在逻辑层。通过数据驱动方式，可以让状态和视图绑定在一起。

WXML 结构实际上等价于一棵 Dom 树，通过一个 JS 对象也可以来表达 Dom 树的结构。在小程序中，WXML 可以先转成 JS 对象，然后再渲染出真正的 Dom 树。通过`setData`把 msg 数据从“Hello World”变成“Goodbye”，产生的 JS 对象对应的节点就会发生变化，此时可以对比前后两个 JS 对象得到变化的部分，然后把这个差异应用到原来的 Dom 树上，从而达到更新 UI 的目的，这就是“数据驱动”的原理。

## 生命周期

![life](C:\Users\frank.hu\Documents\notes\MiniProgram\life.PNG)

- 应用生命周期(`App({})`)

  ```javascript
  App({
    onLaunch: function(options) {},
    onShow: function(options) {},
    onHide: function() {},
    onError: function(msg) {},
    globalData: "I am global data"
  });
  ```

- 页面生命周期(`Page({})`)

  ```javascript
  Page({
    data: { text: "This is page data." },
    onLoad: function(options) {},
    onReady: function() {},
    onShow: function() {},
    onHide: function() {},
    onUnload: function() {},
    onPullDownRefresh: function() {},
    onReachBottom: function() {},
    onShareAppMessage: function() {},
    onPageScroll: function() {}
  });
  ```

## 页面路由

### 页面栈

| 路由方式   | 页面栈表现                        |
| ---------- | --------------------------------- |
| 初始化     | 新页面入栈                        |
| 打开新页面 | 新页面入栈                        |
| 页面重定向 | 当前页面出栈，新页面入栈          |
| 页面返回   | 页面不断出栈，直到目标返回页      |
| Tab 切换   | 页面全部出栈，只留下新的 Tab 页面 |
| 重加载     | 页面全部出栈，只留下新的页面      |

### 路由方式

| 路由方式   | 触发时机                                                                                                                                                                                                                                                    | 路由前页面 | 路由后页面         |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ------------------ |
| 初始化     | 小程序打开的第一个页面                                                                                                                                                                                                                                      |            | onLoad, onShow     |
| 打开新页面 | 调用 API [`wx.navigateTo`](https://developers.weixin.qq.com/miniprogram/dev/api/wx.navigateTo.html) 或使用组件 [`<navigator open-type="navigateTo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)                           | onHide     | onLoad, onShow     |
| 页面重定向 | 调用 API [`wx.redirectTo`](https://developers.weixin.qq.com/miniprogram/dev/api/wx.redirectTo.html) 或使用组件 [`<navigator open-type="redirectTo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)                           | onUnload   | onLoad, onShow     |
| 页面返回   | 调用 API [`wx.navigateBack`](https://developers.weixin.qq.com/miniprogram/dev/api/wx.navigateBack.html) 或使用组件[`<navigator open-type="navigateBack">`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)或用户按左上角返回按钮 | onUnload   | onShow             |
| Tab 切换   | 调用 API [`wx.switchTab`](https://developers.weixin.qq.com/miniprogram/dev/api/wx.switchTab.html) 或使用组件 [`<navigator open-type="switchTab"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) 或用户切换 Tab               |            | 各种情况请参考下表 |
| 重启动     | 调用 API [`wx.reLaunch`](https://developers.weixin.qq.com/miniprogram/dev/api/wx.reLaunch.html) 或使用组件 [`<navigator open-type="reLaunch"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)                                 | onUnload   | onLoad, onShow     |

Tab 切换对应的生命周期（以 A、B 页面为 Tabbar 页面，C 是从 A 页面打开的页面，D 页面是从 C 页面打开的页面为例）：

| 当前页面        | 路由后页面    | 触发的生命周期（按顺序）                           |
| --------------- | ------------- | -------------------------------------------------- |
| A               | A             | Nothing happend                                    |
| A               | B             | A.onHide(), B.onLoad(), B.onShow()                 |
| A               | B（再次打开） | A.onHide(), B.onShow()                             |
| C               | A             | C.onUnload(), A.onShow()                           |
| C               | B             | C.onUnload(), B.onLoad(), B.onShow()               |
| D               | B             | D.onUnload(), C.onUnload(), B.onLoad(), B.onShow() |
| D（从转发进入） | A             | D.onUnload(), A.onLoad(), A.onShow()               |
| D（从转发进入） | B             | D.onUnload(), B.onLoad(), B.onShow()               |

## 事件

- 事件是视图层到逻辑层的通讯方式。
- 事件可以将用户的行为反馈到逻辑层进行处理。
- 事件可以绑定在组件上，当达到触发事件，就会执行逻辑层中对应的事件处理函数。
- 事件对象可以携带额外信息，如 id, dataset, touches。

### 捕获及冒泡

捕获的方向是从最外层到最内层，冒泡的方向是从最内层到最外层。

#### 事件绑定和冒泡

事件绑定的写法同组件的属性，以 key、value 的形式。

- key 以`bind`或`catch`开头，然后跟上事件的类型，如`bindtap`、`catchtouchstart`。自基础库版本 [1.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，在非[原生组件](https://developers.weixin.qq.com/miniprogram/dev/component/native-component.html)中，`bind`和`catch`后可以紧跟一个冒号，其含义不变，如`bind:tap`、`catch:touchstart`。
- value 是一个字符串，需要在对应的 Page 中定义同名的函数。不然当触发事件的时候会报错。

`bind`事件绑定不会阻止冒泡事件向上冒泡，`catch`事件绑定可以阻止冒泡事件向上冒泡。

如在下边这个例子中，点击 inner view 会先后调用`handleTap3`和`handleTap2`(因为 tap 事件会冒泡到 middle view，而 middle view 阻止了 tap 事件冒泡，不再向父节点传递)，点击 middle view 会触发`handleTap2`，点击 outer view 会触发`handleTap1`。

```html
<view id="outer" bindtap="handleTap1">
  outer view
  <view id="middle" catchtap="handleTap2">
    middle view
    <view id="inner" bindtap="handleTap3">inner view</view>
  </view>
</view>
```

#### 事件的捕获阶段

自基础库版本 [1.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，触摸类事件支持捕获阶段。捕获阶段位于冒泡阶段之前，且在捕获阶段中，事件到达节点的顺序与冒泡阶段恰好相反。需要在捕获阶段监听事件时，可以采用`capture-bind`、`capture-catch`关键字，后者将中断捕获阶段和取消冒泡阶段。

在下面的代码中，点击 inner view 会先后调用`handleTap2`、`handleTap4`、`handleTap3`、`handleTap1`。

```html
<view
  id="outer"
  bind:touchstart="handleTap1"
  capture-bind:touchstart="handleTap2"
>
  outer view
  <view
    id="inner"
    bind:touchstart="handleTap3"
    capture-bind:touchstart="handleTap4"
  >
    inner view
  </view>
</view>
```

如果将上面代码中的第一个`capture-bind`改为`capture-catch`，将只触发`handleTap2`。

```html
<view
  id="outer"
  bind:touchstart="handleTap1"
  capture-catch:touchstart="handleTap2"
>
  outer view
  <view
    id="inner"
    bind:touchstart="handleTap3"
    capture-bind:touchstart="handleTap4"
  >
    inner view
  </view>
</view>
```

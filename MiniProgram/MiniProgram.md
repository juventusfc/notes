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

WXS（WeiXin Script）是小程序的一套脚本语言，结合 `WXML`，可以构建出页面的结构。其实就是对JavaScript的封装及限制。

其特点为：

1. wxs 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行。
2. wxs 与 javascript 是不同的语言，有自己的语法，并不和 javascript 一致。
3. wxs 的运行环境和其他 javascript 代码是隔离的，wxs 中不能调用其他 javascript 文件中定义的函数，也不能调用小程序提供的API。
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

## MINA框架

![mina](C:\Users\frank.hu\Documents\notes\MiniProgram\mina.png)

WXML 模板和 WXSS 样式工作在渲染层，JS 脚本工作在逻辑层。通过数据驱动方式，可以让状态和视图绑定在一起。

WXML结构实际上等价于一棵Dom树，通过一个JS对象也可以来表达Dom树的结构。在小程序中，WXML可以先转成JS对象，然后再渲染出真正的Dom树。通过`setData`把msg数据从“Hello World”变成“Goodbye”，产生的JS对象对应的节点就会发生变化，此时可以对比前后两个JS对象得到变化的部分，然后把这个差异应用到原来的Dom树上，从而达到更新UI的目的，这就是“数据驱动”的原理。

## 生命周期

![life](C:\Users\frank.hu\Documents\notes\MiniProgram\life.PNG)

* 应用生命周期(`App({})`)

  ```javascript
  App({
    onLaunch: function(options) {},
    onShow: function(options) {},
    onHide: function() {},
    onError: function(msg) {},
    globalData: 'I am global data'
  })
  ```

* 页面生命周期(`Page({})`)

  ```javascript
  Page({
    data: { text: "This is page data." },
    onLoad: function(options) { },
    onReady: function() { },
    onShow: function() { },
    onHide: function() { },
    onUnload: function() { },
    onPullDownRefresh: function() { },
    onReachBottom: function() { },
    onShareAppMessage: function () { },
    onPageScroll: function() { }
  })
  ```

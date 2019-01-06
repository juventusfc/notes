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

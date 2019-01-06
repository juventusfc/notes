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

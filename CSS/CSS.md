# CSS

## 语言

CSS 的顶层样式表由两种规则组成的规则列表构成，一种被称为 at-rule，也就是 at 规则，另一种是 qualified rule，也就是普通规则，包括选择器和属性。

### @rule

- @charset: 编码方式
- @import: 引入另一个 css 文件

  ```css
  @import [ <url> | <string> ] [ supports(
      [ <supports-condition> | <declaration> ]
    ) ]? <media-query-list>?;
  ```

- @media: 对设备类型进行判断

  ```css
  @media print {
    body {
      font-size: 10pt;
    }
  }
  ```

- @page: 分页媒体访问页面时的表现设置

  ```css
  @page {
    size: 8.5in 11in;
    margin: 10%;

    @top-left {
      content: "Hamlet";
    }
    @top-right {
      content: "Page " counter(page);
    }
  }
  ```

- @counter-style: 定义列表现
- @keyframes: 定义动画关键帧
- @fontface: 定义一种字体
- @supports: 检查环境特性
- @namespace:

### 普通规则

#### 选择器

- combinator
  - 空格: 后代，选中子节点和所有子节点的后代
  - > : 子代，选中子节点
  - \+: 直接后继选择器，选中它的下一个相邻节点，一定要紧挨着
  - ~: 后继，选中它之后的所有相邻节点，不一定要紧挨着
  - ||: 列，选中表格中的一列
- compound-selector
  - type-selector
  - subclass-selector
    - id
    - class
    - attribute
    - pseudo-class
  - pseudo-selector

![css-selector](./images/css-selector.png)

#### 声明列表

声明列表由`属性: 值`组成。

##### 计算型函数

- `calc()`
- `max()`
- `min()`
- `clamp()`
- `toggle()`
- `attr()`

### 单位

## 功能

### 布局

#### 正常流

#### 弹性布局

### 绘制

#### 文字相关

#### 颜色和形状

### 交互

#### 动画

#### 其他交互

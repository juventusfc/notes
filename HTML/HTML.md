# HTML

## 元素

### 文档元信息

Meta 提供 HTML 的辅助型信息。一般在`<head></head>`中。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- charset表示编码集 -->
    <meta charset="UTF-8" />

    <!-- name 表示参数，content 表示参数值 -->
    <!-- 现在为了适配多种设备，需要设置 viewport -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <!-- http-equiv 表示执行一个命令，向浏览器传递一些信息。http-equiv 表示参数，content 表示参数值 -->
    <!-- 如下 meta 表示在 IE 浏览器解析 HTML 时: -->
    <!-- 如果用户版本为IE9，就用IE9；如果用户版本为IE8，就用IE8。 -->
    <!-- 如果content="ie=8",解析的版本最高为IE8，即使用户使用的是 Edge -->
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />

    <!-- head 标签必须有 title -->
    <title>Document</title>
  </head>
  <body></body>
</html>
```

[参考 1](https://www.w3schools.com/tags/tag_meta.asp)
[参考 2](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)

### [语义类标签](./语义类标签.md)

语义类标签有语义，能描述标签的用途，使开发者能一眼就看出来这个标签用处是什么。如`<p></p>`表示 paragraph，`<footer></footer>`表示页尾注脚。但是`<span></span>`等没有，就不是语义类标签。

实际上在使用时，由于开发者对语义类标签不理解或理解方式不一致，会导致语义类标签误导其他开发者。所以，不使用语义类标签，只靠`<div></div>`和`<span></span>`，然后在标签上增加`class="xx"`来表示用途，是以前的很多项目采用的方式。两种方式各有优缺。

### 链接

- `<a></a>`
- `<link></link>`
  一般指向 stylesheet

  ```html
  <head>
    <link href="/media/examples/link-element-example.css" rel="stylesheet">
    <link rel="icon" href="favicon.ico">
  </head>
  ```

- `<nav></nav>`
  导航栏，指向一系列地址

  ```html
  <nav class="crumbs">
    <ol>
        <li class="crumb"><a href="bikes">Bikes</a></li>
        <li class="crumb"><a href="bikes/bmx">BMX</a></li>
        <li class="crumb">Jump Bike 3000</li>
    </ol>
  </nav>
  ```

### 替换型元素

### 表单

### 表格

### 总集

## 语言

### 实体

### 命名空间

## 补充标准

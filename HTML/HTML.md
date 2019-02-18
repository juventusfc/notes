# HTML

## Meta

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

    <!-- http-equiv 向浏览器传回一些信息。http-equiv 表示参数，content 表示参数值 -->
    <!-- 如下 meta 表示在 IE 浏览器解析 HTML 时: -->
    <!-- 如果用户版本为IE9，就用IE9；如果用户版本为IE8，就用IE8。 -->
    <!-- 如果content="ie=8",解析的版本最高为IE8，即使用户使用的是 Edge -->
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body></body>
</html>
```

[参考 1](https://www.w3schools.com/tags/tag_meta.asp)
[参考 2](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)

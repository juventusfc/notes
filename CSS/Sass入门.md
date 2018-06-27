# Sass 入门

Sass 是 css 的预处理器，是 css 的一种扩展。在工程中，先写 scss 文件，然后用 webpack 等工具将 scss 文件转换为 css 文件。Sass 官网的文档比较详细也比较易懂，现在将基础知识归纳如下：

## Scss 与 sass 的关系

Scss 是 sass 的一种语法形式。建议使用 scss.

## Variables

```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

## Nesting

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li {
    display: inline-block;
  }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

## Partials 使用 import

```scss
// \_reset.scss

html,
body,
ul,
ol {
  margin: 0;
  padding: 0;
}

// base.scss

@import "reset";

body {
  font: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```

## Mixins

```scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  -ms-border-radius: $radius;
  border-radius: $radius;
}

.box {
  @include border-radius(10px);
}
```

## Extend/Inheritance

```scss
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.message {
  @extend %message-shared;
}
```

## Operators

```scss
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 600px / 960px \ * 100%;
}
```

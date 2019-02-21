# HTML

## 元素

### Meta

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

### 语义类标签

语义类标签有语义，能描述标签的用途。如`<p></p>`表示 paragraph，但是`<span></span>`等没有，就不是语义类标签。当然，不使用语义类标签，只靠`<div></div>`和`<span></span>`，是以前的很多项目采用的方式。两种方式各有优缺。
Pros：

- 语义类标签对开发者更为友好，使用语义类标签增强了可读性，即便是在没有 CSS 的时候，开发者也能够清晰地看出网页的结构，也更为便于团队的开发和维护。
- 除了对人类友好之外，语义类标签也十分适宜机器阅读。它的文字表现力丰富，更适合搜索引擎检索（SEO），也可以让搜索引擎爬虫更好地获取到更多有效信息，有效提升网页的搜索量，并且语义类还可以支持读屏软件，根据文章可以自动生成目录等等。

Cons：

- 开发人员对语义类标签理解不一致会导致 HTML 结构更混乱。

#### 作为自然语言延伸的语义类标签

```html
<ruby>
漢 <rt> ㄏㄢˋ </rt>
</ruby>
```

#### 作为标题摘要的语义类标签

```html
<!-- 使用 hgroup 来确定副标题 -->
<hgroup>
<h1>JavaScript 对象 </h1>
<h2> 我们需要模拟类吗？</h2>
</hgroup>
<p>balah balah</p>

<!-- 使用 section 来消除使用h2~hn -->
<section>
    <h1>HTML 语义 </h1>
    <p>balah balah balah balah</p>
    <section>
        <h1> 弱语义 </h1>
        <p>balah balah</p>
    </section>
    <section>
        <h1> 结构性元素 </h1>
        <p>balah balah</p>
    </section>
</section>
```

#### 作为整体结构的语义类标签

```html
<body>
    <header>
        <nav>
            ……
        </nav>
    </header>
    <aside>
        <nav>
            ……
        </nav>
    </aside>
    <section>……</section>
    <section>……</section>
    <section>……</section>
    <footer>
        <address>……</address>
    </footer>
</body>
```

```html
<body>
    <header>……</header>
    <article>
        <header>……</header>
        <section>……</section>
        <section>……</section>
        <section>……</section>
        <footer>……</footer>
    </article>
    <article>
        ……
    </article>
    <article>
        ……
    </article>
    <footer>
        <address></address>
    </footer>
</body>
```

#### HTML5 中新增的语义类标签

| Tag                                                             | Description                                                                                 |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| [article](https://www.w3schools.com/tags/tag_article.asp)       | Defines an article                                                                          |
| [aside](https://www.w3schools.com/tags/tag_aside.asp)           | Defines content aside from the page content                                                 |
| [details](https://www.w3schools.com/tags/tag_details.asp)       | Defines additional details that the user can view or hide                                   |
| [figcaption](https://www.w3schools.com/tags/tag_figcaption.asp) | Defines a caption for a `<figure>` element                                                  |
| [figure](https://www.w3schools.com/tags/tag_figure.asp)         | Specifies self-contained content, like illustrations, diagrams, photos, code listings, etc. |
| [footer](https://www.w3schools.com/tags/tag_footer.asp)         | Defines a footer for a document or section                                                  |
| [header](https://www.w3schools.com/tags/tag_header.asp)         | Specifies a header for a document or section                                                |
| [main](https://www.w3schools.com/tags/tag_main.asp)             | Specifies the main content of a document                                                    |
| [mark](https://www.w3schools.com/tags/tag_mark.asp)             | Defines marked/highlighted text                                                             |
| [nav](https://www.w3schools.com/tags/tag_nav.asp)               | Defines navigation links                                                                    |
| [section](https://www.w3schools.com/tags/tag_section.asp)       | Defines a section in a document                                                             |
| [summary](https://www.w3schools.com/tags/tag_summary.asp)       | Defines a visible heading for a `<details>` element                                         |
| [time](https://www.w3schools.com/tags/tag_time.asp)             | Defines a date/time                                                                         |

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

# AEM develop

## Install

1. Copy& paste aem-install.jar and lisence to a folder, ex: `/AEM6.4/author/`
2. Rename aem-instll to cq-author-p4502.jar. Author means instance name, p4502 means port
3. Start author instance by double clicking cq-author-p4502.jar

## Eclipse 代码与 AEM 的同步

Eclipse 中，AEM 视图，

- Server 点击 Publish，将 Eclipse 代码部署到 AEM（也可以使用 Maven，语句为`mvn -PautoInstallPackage -Padobe-public clean install`）
- 代码目录上，点击 import from AEM，将在 AEM 进行的操作同步回 Eclipse 中。一般只会同步用户在 AEM 中建立的 Template 和页面内容。

## 在 eclipse 中展示.content.xml 文件

`Project Explore`中点击三角形 --> `Filters And Customization` --> `uncheck *.resources`

## css/js 部署到 AEM

- 使用手工部署方式

  1. 在 Eclipse 中修改 css/js
  2. 使用 maven 部署到 AEM
  3. 在 publish 模式下查看 css/js 有没有生效

  如果页面显示不正确，则需要[查看 css/js](http://localhost:4502/libs/granite/ui/content/dumplibs.html)和[清理缓存(推荐方式)或重新生成(耗时较长，一般不推荐)](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)

- 使用 aemfe 自动部署
  1. 项目根目录下执行`aemfed -t "http://admin:admin@localhost:4502" -e "**/*___jb_+(old|tmp)___" -w "ui.apps/src/main/content/jcr_root/"`
  2. 在 Eclipse 中修改 css/js
  3. aemfe 自动部署至 AEM
  4. 在 publish 模式下查看 css/js 有没有生效

在开发过程中，由于缓存，经常会发生 css 样式没有更新的现象。一般解决这种问题的步骤是：

1. 查看 Lite 中代码是否有同步过来
2. [查看生成的 css/js](http://localhost:4502/libs/granite/ui/content/dumplibs.html)
3. 如果第二步生成的 css/js 不正确，[清理缓存(推荐方式)或重新生成(耗时较长，一般不推荐)](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)
4. 查看 aemfe 代理页面有没有更新 css。如果没有更新，`Ctrl + F5`强制刷新
5. 查看真正的页面（被代理页面）。如果没有更新，`Ctrl + F5`强制刷新

## 整体架构

AEM 基于 Granite 构建，同时融入了 Sling 和 JCR 技术。

![high-level](./images/high-level.png)

![granite](./images/granite.png)

Granite 包含很多基础模块。其中，OSGI 的实现采用了 Felix 项目。类似于.net 项目中的自带基础功能。

Sling 是一种 Web Application Framework。类似于.net 项目中的 MVC 框架，将请求的 URI 映射为 JCR 中的 node(也就是 Resource)。

JCR 是一种内容数据库，用于存储数据。类似于.net 项目中的使用的 SQL Server。

## 自带工具

1. Web Console
   用于查看 OSGI/Sling 等。一些 AEM 的基本配置也在这里配置。
2. CRXDE Lite
   用于编辑 JCR 节点上的内容。
3. Packages
   用于将 JCR 通过 vault 转换为 File System 形式。
   // TODO 需要学习 JCR 与 File System 的[对应关系](http://jackrabbit.apache.org/filevault/vaultfs.html)

PS:根据教程，在这里通过 Package 上传了 SamplePackage.zip

## node 和 property

node 决定了 JCR 的结构层次，property 决定了 node 的属性。

node 有一个重要属性 jcr:primaryType。它决定了 node 的基本类型，常用的基本类型有 cq:Component 等。常用的类型可查询[官网](https://helpx.adobe.com/in/experience-manager/6-4/sites/developing/using/custom-nodetypes.html)。

官网中的 definition 可参考[Node Type Annotation](http://jackrabbit.apache.org/jcr/node-type-notation.html)

```json
/*  An example node type definition */

// The namespace declaration
<ns = 'http://namespace.com/ns'>

// Node type name
[ns:NodeType]

// Supertypes
> ns:ParentType1, ns:ParentType2

// This node type supports orderable child nodes
orderable

// This is a mixin node type
mixin

// Nodes of this node type have a property called 'ex:property' of type STRING
- ex:property (string)

// The default values for this
// (multi-value) property are...
= 'default1', 'default2'

// This property is the primary item
primary

// and it is...
mandatory autocreated protected

// and multi-valued
multiple

// It has an on-parent-version setting of ...
version

// The constraint settings are...
< 'constraint1', 'constraint2'

// Nodes of this node type have a child node called ns:node which must be of
// at least the node types ns:reqType1 and ns:reqType2
+ ns:node (ns:reqType1, ns:reqType2)

// and the default primary node type of the child node is...
= ns:defaultType

// This child node is...
mandatory autocreated protected

// and supports same name siblings
multiple

// and has an on-parent-version setting of ...
version
```

## 渲染过程

PS: XX/ means XX folder

1. Create folders
   1. Create training/ under apps/
   2. Create components/ and templates/ under training/
   3. Create content/ and structure/ under components/
2. Create a Component
   1. Right click components/, create component `contentpage`
   2. Using a html as a default rendering script. The name of html file should be the same as component name
3. Create a content node
   1. Under content/, create node `hello-world`
   2. Add `sling:resourceType = training/components/structure/contentpage` as a node property
4. Render content
   1. In browser, using `http://localhost:4502/content/hello-world.html` to render the Component

当在浏览器中输入 URL 时，`/content/hello-world.html`指向 JCR 中的`/content/hello-world`节点。在这个节点上，`sling:resourceType`指向`training/components/structure/contentpage`这个 Component。然后就会渲染这个 Component 的 Render Script，返回给浏览器。

具体的步骤为：

1. Decompose the URL

   ![decompose-url](./images/decompose-url.png)

2. Search for servlet or vanity URL redirect
3. Search for a node indicated by the URL
4. Resolve the resource

   ![resolve-request](./images/resolve-request.png)

5. Resolve the rendering script/servlet
6. Create rendering chain
7. Invoke rendering chain

![url-rernder-all](./images/url-rernder-all.png)

注意，图中的数字编号与上面描述不匹配。

## Template

Template 用于创建 Page。在创建 Template 的时候，会指定 Tempalte 的`sling:resourceType`指向某个 Component。在创建 Page 时，会将 Template 的`sling:resourceType`属性复制给 Page(整个 Template 下的`jcr:content`节点都会被复制)，从而这个 Page 在 Render 的时候能找到对应 Component 的 Render Script(参考渲染过程)。

### 限制使用 Template

- 在 Template 层面，使用`allowedPaths`属性来限制使用
- 在 Content 层面，使用`cq:allowedTemplates`属性来限制使用

### 优化 Template

以增加 Thumbnail 为例

1. 在 Page 上将需要的 Thumbnail 上传
2. 在 CRXDE 中复制 Page/jcr:content/image 节点，将该节点粘贴至 templates/contactpage/jcr:content 节点下。

## HTL

HTL 是一种模板语言。在服务器端，通过解析 HTL 然后返回 HTML 给浏览器。类似于.net 项目中的 Razor Page(cshtml)。

- Block Statements
  `data-sly-*`
- Expressions
  `${}`

```html
  <!doctype html>
  <html>
    <head>
        <meta charset="utf-8"/>
    </head>
    <body>
        <h1>Hello World!!</h1>
        <h3>Sling PropertiesObject</h3>
        <!-- properties 指向当前 Resource -->
        <p>Page Title : ${properties.jcr:title}</p>

        <h3>Page Details</h3>
        <!-- currentPage 是 Java 对象，真实的方法是 getXX()，实际调用时只需写 ${currentPage.XX} -->
        <p>currentPage Title: ${currentPage.Title}</p>
        <p>currentPage Name: ${currentPage.Name}</p>
        <p>currentPage Path: ${currentPage.Path}</p>
        <p>currentPage Depth: ${currentPage.Depth}</p>

        <h3> Node Details </h3>
        <p>currentNode Name: ${currentNode.Name}</p>
        <p>currentNode Path: ${currentNode.Path}</p>
        <p>currentNode Depth: ${currentNode.Depth}</p>
    </body>
  </html>
```

`data-sly-include`用于将另一部分内容包含在当前内容。

```html
<!-- contactpage.html -->
<div data-sly-include="body.html"></div>

<!-- body.html -->
<div class="container we-Container--main"></div>

<!-- 最后产生的页面 -->
<div>
    <div class="container we-Container--main"></div>
</div>
```

## 继承

### 3 种层次关系

- Resource Type 层次关系
  通过 sling:resourceSuperType 属性来决定继承关系
- Container 层次关系
  主要用于给子 Component 配置，常用于 cq:editConfig 和 cq:childEditConfig 属性
- Include 层次关系
  主要用于运行时

// TODO data-sly-resource 和 data-sly-include 的区别

### 继承关系的解释

当子类渲染时，通过 sling:resourceSuperType 关联至父类。渲染原则是**子类优先**。

1. 当子类存在对应的 Init Script 时，执行子类的 Init Script
   当子类的 Init Script 调用了父类和子类都有的 Script，优先调用子类的
2. 当子类没有对应的 Init Script，执行父类的 Init Script
   当父类的 Init Script 调用了父类和子类都有的 Script，优先调用子类的

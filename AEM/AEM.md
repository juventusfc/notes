# AEM develop

## Install

1. Copy& paste aem-install.jar and lisence to a folder, ex: `/AEM6.4/author/`
2. Rename aem-instll to cq-author-p4502.jar. Author means instance name, p4502 means port
3. Start author instance by double clicking cq-author-p4502.jar

## 开发工具

### IntelliJ

IntelliJ 主要用于 AEM 中的 Java 开发。
`mvn -PautoInstallPackage -Padobe-public clean install` 部署至 AEM。

#### Debug

1. 命令行中启动 cq-author-p4502.jar:
   `java -jar cq-author-p4502.jar -fork -forkargs -- -Xdebug -Xrunjdwp:transport=dt_socket,address=59865,suspend=n,server=y -Xmx1520m -XX:MaxPermSize=512m -XX:-UseSplitVerifier`
2. IntelliJ 中，
   1. Go to **Run**->**Edit Configurations**
   2. Click on **+** and choose **Remote**
   3. Add **Name** and modify **Port** to 59865
   4. Go to **Run**-> **Debug**-> **Choose your configuration**
   5. Set breakpoint to code and enjoy your debugging
3. 当要修改代码并重新 debug 时，需要先将修改后的代码同步至 AEM

### Brackets

Brackets 主要用 AEM 中前端的开发。还要安装 AEM Brackets Extension 用以与 AEM 中的代码同步。

### Eclipse

插件不稳定，Eclipse 自身比较卡，不推荐。

#### Eclipse 代码与 AEM 的同步

Eclipse 中，AEM 视图，

- Server 点击 Publish，将 Eclipse 代码部署到 AEM（也可以使用 Maven，语句为`mvn -PautoInstallPackage -Padobe-public clean install`）
- 代码目录上，点击 import from AEM，将在 AEM 进行的操作同步回 Eclipse 中。一般只会同步用户在 AEM 中建立的 Template 和页面内容。

#### 在 eclipse 中展示.content.xml 文件

`Project Explore`中点击三角形 --> `Filters And Customization` --> `uncheck *.resources`

#### css/js 部署到 AEM

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

![aem-archi](./images/aem-archi.png)

![granite](./images/granite.png)

Granite 包含很多基础模块，比如 Granite UI 等。

- Sling 是一种 Web Application Framework，用于处理 REST 请求。类似于.net 项目中的 MVC 框架，将请求的 URL 映射为 JCR 中的 node(也就是 Resource)。

- JCR 是一种内容数据库标准，用于存储数据，AEM 的实现采用了 Apache Jackrabbit Oak 项目，在 AEM 里叫 CRX Content Repository。类似于.net 项目中的使用的 SQL Server。

- OSGi 是一种 Bundle 技术，AEM 的实现采用了 Apache Felix 项目。

## 搭建框架

1. 推荐使用 Maven 搭建。

   ```bash
   mvn org.apache.maven.plugins:maven-archetype-plugin:2.4:generate \
   -DarchetypeGroupId=com.adobe.granite.archetypes \
   -DarchetypeArtifactId=aem-project-archetype \
   -DarchetypeVersion=15 \
   -DarchetypeCatalog=https://repo.adobe.com/nexus/content/groups/public/
   ```

2. 使用 Git 管理

   ```bash
   git init
   ```

3. 搭建后，导入 IntelliJ 和 Brackets 中就可以进行开发了。

## OSGi

OSGi 包含 Component 和 Service。

1. 新建 service 接口
2. 新建 component，并将 component 注册为一个 service，注册的 service 就是第一步新建的 service
3. [console-components](http://localhost:4502/system/console/components/) 和 [console-services](http://localhost:4502/system/console/services/) 中查看是否生效。

## 自带工具

1. Web Console
   用于查看 OSGI/Sling 等。一些 AEM 的基本配置也在这里配置。
2. CRXDE Lite
   用于编辑 JCR 节点上的内容。
3. Packages
   用于将 JCR 通过 vault 转换为 File System 形式。
   JCR 与 File System 的[对应关系](http://jackrabbit.apache.org/filevault/vaultfs.html)

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

### data-sly-include

`data-sly-include`主要用于同一个组件内引用其他 html，用于将另一部分内容包含在当前内容。

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

### data-sly-resource

`data-sly-resource` 主要用于引用其他 Content Component

```html
<!-- header.html -->
<!-- site-topnav 是一个 Component -->
<div data-sly-resource="${'site-topnav' @ resourceType='training/components/structure/site-topnav'}"></div>
```

### data-sly-use

`data-sly-use`用于使用 Java 或 JavaScript 来实现复杂逻辑。

- Java

  ```html
  <div data-sly-use.topnav="TopNav" class="container we-Container--top-navbar"></div>
  ```

- JavaScript

  ```html
  <div data-sly-use.topnav="topNav.js" class="container we-Container--top-navbar"></div>
  ```

## 继承

### 3 种层次关系

- Resource Type 层次关系
  通过 sling:resourceSuperType 属性来决定继承关系
- Container 层次关系
  主要用于给子 Component 配置，常用于 cq:editConfig 和 cq:childEditConfig 属性
- Include 层次关系
  主要用于运行时

### 继承关系的解释

当子类渲染时，通过 sling:resourceSuperType 关联至父类。渲染原则是**子类优先**。

1. 当子类存在对应的 Init Script 时，执行子类的 Init Script
   当子类的 Init Script 调用了父类和子类都有的 Script，优先调用子类的
2. 当子类没有对应的 Init Script，执行父类的 Init Script
   当父类的 Init Script 调用了父类和子类都有的 Script，优先调用子类的

## ClientLib

1. 定义 ClientLib，并定义 categories
2. 在 Compoent 上使用`<sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html" data-sly-call="${clientLib.(js|css) @ categories='we.train.all'}"/>`来调用 js 或 css

## Logging

1. Click Web Console--Sling--Log Support
2. Add new Logger then Save
3. Java/JavaScript

   ```java
   //Logger Object
   private static final Logger LOGGER = LoggerFactory.getLogger(TopNav.class);
   //Logger Message
   LOGGER.info("########[JAVA] Root page is: " + rootPage.getTitle());
   ```

   ```javascript
   //Logging Message
   log.info("########[JS] Root page is: {}", root.getTitle());
   ```

4. Check logger under logs folder

## Dialog

### cq:dialog

分为 Touch UI 和 Classic UI。Touch UI 基于 granite.js，所有可用在 Dialog 里的在`/libs/granite/ui/components`下，Dialog(cq:dialog) 的 sling:resourceType 是`cq/gui/components/authoring/dialog`。

#### 增加 cq:dialog 后的渲染过程

项目状态

- `/content/train/en/about`页面的 sling:resourceType 为`training/components/structure/contentpage`
- `/apps/training/components/structure/contentpage` contentpage Component 中的 Script 引用了 `<div class="we-Header" data-sly-resource="${'title' @ resourceType='training/components/structure/title'}"></div>`
- `/apps/training/components/structure/title` title Component 中的 Dialog 中，指定了 title 的存储位置为`./jcr:title`

`http://localhost:4502/editor.html/content/train/en/about.html` 渲染过程

1. Author 在 About 页面配置 title Component 为`hello Title`
   ![render-01](./images/render-01.png)
2. 点击保存后，title Component 中的 Dialog 中，指定了 title 的存储位置为`./jcr:title`，所以在`/content/train/en/about/jcr:content`下新增 title 节点
   ![render-02](./images/render-02.png)
3. 页面重新加载，找到 contentpage Component，碰到`<div class="we-Header" data-sly-resource="${'title' @ resourceType='training/components/structure/title'}"></div>`
4. 渲染 title Component，`<h1 data-sly-use.title="title.js">${title.text}</h1>`，在 title.js 中`title.text = properties.get(CONST.PROP_TITLE) || pageProperties.get(CONST.PROP_TITLE) || currentPage.name;`,properties 表示当前 resource，也就是`/content/train/en/about/jcr:content/title`,里面存的值是`hello Title`
5. 页面最终效果
   ![render-03](./images/render-03.png)

### cq:editConfig

用于增强 dialog 的功能。

### cq:design_dialog

用于修改全局 content。`cq:design_dialog`来充当 dialog。`etc/designs`存放内容。在`design mode`下打开 dialog。

### cq:dialog & cq:editConfig & cq:design_dialog

- cq:dialog 用于当前 Component 级别的输入。只在当前页面起作用。
- cq:editConfig 用于对 cq:dialog 的增强
- cq:design_dialog 类似于 cq:dialog，但是是在 design mode 下打开，一般在根页面上配置目录，然后在某个页面的 Component 上在 design mode 下调样式或其他，会在全部页面上的对应 Component 上起作用

## Response Grid

### 1. 增加 responsivegrid

1. 在需要 responsivegrid(layout container) 的 Component 上增加
   `<div data-sly-resource="${'responsivegrid' @ resourceType='wcm/foundation/components/responsivegrid'}"></div>`
2. 点击右上角，切换到 design 模式
3. 点击 responsivegrid 上的 parent
4. 点击 responsivegrid 上的 configure
5. 选择可以添加到 responsivegrid 的 component
6. 点击右上角，切换回 edit 模式

### 2. 编辑页面增加 Emulator

在页面节点(jcr:content) 上配置属性`cq:deviceGroups="/etc/mobile/groups/responsive"`

### 3. 允许页面 responsive，在页面右上角出现 layout 模式

在页面节点(jcr:content) 下新增`cq:responsive`节点

// TODO parsys && iparsys && responsivegrid 对比

## Sling

### 在 HTL 中增加 selectors

1. 新增 Render Script
2. `<form class="page__print" action="${currentPage.Path @ selectors='print'}.html"> <input value="Print Friendly" type="submit" /> </form>` 传入 selectors

### Sling Resource Merger

Sling Resource Merger 用于 AEM 的覆盖和继承。

- overlay
  复制 lib/下的 Component 到 apps/下。由于 AEM 自带的 search path 会先从 apps/寻找，apps/下的 Component 会先起作用。AEM6.0 之后，Sling Resource Merger 在 overlay 上起作用，不需要 1：1 复制 lib/下某个 Component 的所有节点和属性。

  ```bash
  /libs/sling/example (nt:folder)
     +-- sling:resourceType = "some/resource/type"
     +-- child1 (nt:folder)
     |   +-- property1 = "property from /libs/sling/example/child1"
     +-- child2 (nt:folder)
     |   +-- property1 = "property from /libs/sling/example/child2"
     +-- child3 (nt:folder)
     |   +-- property1 = "property from /libs/sling/example/child3"

  /apps/sling/example (sling:Folder)
     +-- property1 = "property added in apps"
     +-- child1 (nt:folder)
     |   +-- sling:hideResource = true // 隐藏！
     +-- child2 (nt:folder)
     |   +-- property1 = "property from /apps/sling/example/child2"
     +-- child3 (nt:folder)
     |   +-- property2 = "property from /apps/sling/example/child3"

  /mnt/overlay/sling/example (sling:Folder)
     +-- sling:resourceType = "some/resource/type"
     +-- property1 = "property added in apps"
     +-- child2 (nt:folder)
     |   +-- property1 = "property from /apps/sling/example/child2"
     +-- child3 (nt:folder)
     |   +-- property1 = "property from /libs/sling/example/child3"
     |   +-- property2 = "property from /apps/sling/example/child3"
  ```

- override
  通过使用`sling:resourceSuperType`属性来继承父类 Component。

  ```bash
  /apps/sling/base (nt:folder)
     +-- child1 (nt:folder)
     |   +-- property1 = "property from /libs/sling/example/child1"
     +-- child2 (nt:folder)
     |   +-- property1 = "property from /libs/sling/example/child2"
     +-- child3 (nt:folder)
     |   +-- property1 = "property from /libs/sling/example/child3"

  /apps/sling/example (sling:Folder)
     +-- sling:resourceSuperType = "/apps/sling/base"
     +-- property1 = "property added in /apps/sling/example"
     +-- child1 (nt:folder)
     |   +-- sling:hideResource = true // 隐藏！
     +-- child2 (nt:folder)
     |   +-- property1 = "property from /apps/sling/example/child2"
     +-- child3 (nt:folder)
     |   +-- property2 = "property from /apps/sling/example/child3"

  /mnt/override/apps/sling/example (sling:Folder)
     +-- sling:resourceSuperType = "/apps/sling/base"
     +-- property1 = "property added in /apps/sling/example"
     +-- child2 (nt:folder)
     |   +-- property1 = "property from /apps/sling/example/child2"
     +-- child3 (nt:folder)
     |   +-- property1 = "property from /libs/sling/example/child3"
     |   +-- property2 = "property from /apps/sling/example/child3"
  ```

[参考文章](https://aemvardhan.wordpress.com/2017/02/22/understand-aem-sling-resource-merger-override-and-overlay-concepts/)

### Redirect

在页面下的 jcr:content 节点上新增`sling:redirectStatus`和`sling:redirect`和`redirectTarget`

## Internationalization

1. 新建一个 fr 的页面，该 fr 为识别符和路径
2. 在 Component 的 HTL 里修改为`${"&copy; {0} We.Train. All rights reserved." @ i18n, format='2017', context='html'}`。其中，`@i18n`表示使用国际化
3. 在 Component 下新增`i18n`节点，在`i18n`节点下新增`fr`节点。`fr`节点新增`jcr:mixTypes="mix:language"`和`jcr:language="fr"`属性
4. 在[i18n-translator](http://localhost:4502/libs/cq/i18n/translator.html)网页上，找到`/apps/training/components/structure/contentpage/i18n`这个节点，点击`Add`，增加翻译并保存
5. 此时，在`i18n`节点上刷新，应该能看到新增的翻译
6. 打开 fr 页面，看到翻译后的语言

## Debug and Testing

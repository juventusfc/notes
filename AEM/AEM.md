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

Sling 是一种 web application framework。类似于.net 项目中的 MVC 框架，将请求的 URI 映射为 JCR 中的 node(也就是 Resource)。

JCR 是一种内容数据库，用于存储数据。类似于.net 项目中的使用的 SQL Server。

# AEM 开发

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

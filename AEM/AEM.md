# AEM Develop Guides

## Tips

开发一个 Component 的流程

1. 参考 OOTB 的写法
   - foundation/components/text （JSP）
   - wcm/foundation/components/text （HTL）
   - core/wcm/components/text/v2/text （Core Component）
2. 参考 OOTB 中的 cq:dialog 写法
   - 一般 OOTB 的 cq:dialog 的 `sling:resourceType=cq/gui/components/authoring/dialog`
   - 建议使用 `sling:resourceType=granite/ui/components/coral/foundation/*`
   - 如果包含图片/文件上传，使用`sling:resourceType=cq/gui/components/authoring/dialog/fileupload`

Sling Model 能将 Resource 映射为 POJO ，能应用在：

1. 直接在 HTL 中使用，HTL 所在得节点对应的 Resouce 映射为 Sling Model
2. 在 OSGi 的 Service 里使用。在 Service 中获得 Resource 并映射为 Sling Model

## [安装](./01-Install.md)

## [开发工具](./02-Tools.md)

## [整体架构](./03-Architecture.md)

## [项目搭建](./04-Setup.md)

## [AEM Package Structure](./05-package-structure.md)

## [OSGi](./06-OSGi.md)

## [Confuiguring AEM](./07-config.md)

## [Logging](./08-Logging.md)

## [Sling](./06-Sling.md)

## [JCR](./07-JCR.md)

## [Granite/CoralUI](./08-Granite-CoralUI.md)

## [AEM APIs](./09-AEMAPIs.md)

## Tests

## Content Ingestion

## Use && Group && Permissions

# Setup

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

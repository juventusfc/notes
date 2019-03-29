# Install

## What Do I need

- Oracle JDK 1.8
- AEM jar and lisence

## Steps

1. Copy& paste aem-install.jar and lisence to a folder, ex: `/AEM6.4/author/`
2. Rename aem-instll to cq-author-p4502.jar. Author means instance name, p4502 means port
3. Start author instance by double clicking cq-author-p4502.jar

## Setting AEM Run Modes

- Using a system property in the start script
  -Dsling.run.modes=publish,prod,us
- Using the sling.properties file
  1. Edit the configuration file
     `<cq-installation-dir>/crx-quickstart/conf/sling.properties`
  2. Add the following properties; the following example is for author
     sling.run.modes=author
- Using the -r option
  java -jar cq-56-p4545.jar -r dev
- **Filename detection - renaming the jar file**(Prefer this)
  `cq-<run-mode>-p<port-number>`

# Logging

- Logging in AEM is provided by SLF4J (Simple Logging Facade for Java)
- There are five log levels: TRACE, DEBUG, INFO, WARN, ERROR
- Logs are saved to [AEM_INSTALL_DIR]/crx-quickstart/logs

## Logging Best Practices

- Use SLF4J, not System.outor e.printStackTrace()
- Log messages should be in the form of sentences
- Log messages should have context, including exceptions and variables
- All exceptions should have a log message

## Log Levels

- TRACE
- DEBUG
- INFO
- WARN
- ERROR

## Logging Parameters

- Objects can be passed as parameters to log messages
- {} tokens will be replaced with toString() output
- Exceptions will log full stack trace

## Logging in OSGiServices

- Define a Logger:
  `private static final Logger log = LoggerFactory.getLogger(Service.class);`
- Each logger should be private for a service
- Log writers can be configured at the package level

## Configuring Custom Loggers

### Logger Configuration Options

- Sling Loggers OSGi Console  
  Set per instance  
   Useful for local debugging
- OSGi Configuration Node  
  Set globally or per environment  
   Used for general configuration

### Configuring a Logger

- Create an OSGi Configuration called
  - org.apache.sling.commons.log.LogManager.factory.config-[project]
- Set the following settings
  - org.apache.sling.commons.log.name-the packages to log
  - org.apache.sling.commons.log.file-the relative file to log
  - org.apache.sling.commons.log.level-The log level to log

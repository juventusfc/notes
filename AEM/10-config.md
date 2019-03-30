# Confuiguring AEM

## Configurations in AEM

### What is Configurations in AEM

- Provided by OSGi Framework
- Bound to OSGi Components
- Configurable on per environment or per instance

### How to use

[OSGi Configuration Console](http://localhost:4502/system/console/configMgr)

Lists each configurable OSGi Component
Not all OSGi Components have configurations
Console changes saved as config nodes

## Configuring OSGi Services

### OSGi Service Configuration Options

- Static
  - Used for constant settings and framework configurations
  - Set in Component Annotation
  - Not configurable in Console
  - Retrieved from Map in ComponentContext
- Dynamic
  - Used for dynamic settings and application configurations
  - Set in separate @ObjectClassDefinitioninterface
  - Configurable in console
  - Retrieved from dedicated interface

#### Static Configurations

- Set in the property attribute of the @Component annotation
- Set as an array of strings, format: “KEY<:TYPE>=VALUE”
- Set type for non-String types, supported types: Boolean, Byte, Short,Integer, Long, Float, Double, Character
- For multi-properties add multiple values with the same key

#### Dynamic Configurations

- Configurations retrievable from separate configuration interface
- Indicate configuration class with: @Designate(ocd = Configuration.class)
- Configuration interface annotation: @ObjectClassDefinition(name = "My Service Configuration”)
- Properties are interface methods with @AttributeDefinition annotations

  - name

    Sets the label for the field

  - cardinality

    - MIN_VALUE=limitless Vector
    - < 0=limited Vector
    - 0=1 value
    - \> 0=limited array
    - MAX_VALUE=limitless array

  - type

    BOOLEAN, BYTE, CHARACTER,DOUBLE, FLOAT, INTEGER,LONG, SHORT, STRING

## Storing Configurations

### OSGi Config Node Format

- Stored under /apps/[NAME]/config
- Name of node matches OSGi Component PID
- Stored as sling:OsgiConfig or nt:file with the name [OSGi-Component-PID].config

### Overriding OOTB Configurations

- Update Configuration in OSGi Configuration Console
- Locate config under /apps/cq/core/config or /apps/system/config
- Add configuration to project

## Using RunModes + AEM Configurations

### Using RunModes and OSGi Configurations

- Enables setting different configurations per environment or instance
- Stored under /apps/[NAME]/config.[RUN_MODE]
- Multiple Run Modes can be set

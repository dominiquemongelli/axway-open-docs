{
"title": "Entity types in YAML configuration",
"linkTitle": "Entity types in YAML configuration",
"weight":"110",
"date": "2020-09-25",
"description": "Learn how entity types are described in a YAML configuration."
}

The `types` directory located under the `META-INF` directory of a YAML configuration contains the definition of all the entity types in the Entity Store model, and its subdirectories are organized in a hierarchical tree structure that represents the inheritance relationships between types.

An [entity type](/docs/apigtw_devguide/entity_store/#entity-types) is a description of an entity in the Entity Store.

The YAML Entity Store supports all entity types and custom types.

## Simple type

```yaml
name: JMSSession                       # name used in YAML entity file
version: 5
class: com.vordel.dwe.jms.JMSSession
constants:
  descriptorClass:
    type: string
    value: com.vordel.client.manager.filter.jms.JMSTransportDescriptor
fields:
  cloneCount:
    type: integer
    defaultValues:
    - data: 1                          # an example a defaulted field (mandatory but having a default value)
    cardinality: 1
  duplicatesOK:
    type: boolean
    defaultValues:                     # this is an optional field.
    - {}
    cardinality: '?'
  messageRemovalPolicy:
    type: string
    defaultValues:
    - data: UNLESS_EXCEPTION           # if you do specify this field in you YAML file, value will be 'UNLESS_EXCEPTION'
    cardinality: 1
  messageRemovalProperty:
    type: string
    defaultValues:
    - data: jms.message.remove
    cardinality: '?'
  name:                                # this one a mandatory field -- it is actually a key field
    type: string
    defaultValues:
    - {}
    cardinality: 1
  servicePK:                           # this fields must contain a reference to another entity of type 'JMSService'
    type: '@JMSService'                # '@' char tells it is a reference
    cardinality: 1
components:
  JMSConsumer: '?'                     # an entity of this type JSMSession can have 1 children of type JMSConsumer
keyFields:
- name
loadorder: 1000100
```

{{% pageinfo color="primary" %}}
`{}` is YAML syntax for an empty list.
{{% /pageinfo %}}

## Types with inheritance

Consider the following types: `Process`, `JavaProcess` and `NetService`:

```yaml
name: Process
version: 0
fields:
  name:
    type: string
    defaultValues:
    - {}
    cardinality: 1
keyFields:
- name
abstract: true
```

```yaml
name: JavaProcess
version: 0
abstract: true
```

```yaml
name: NetService
version: 5
constants:
  executableImage:
    type: string
    value: vshell
components:
  LoadableModule: '?'
  ClassLoader: '?'
```

and, the following directory structure:

![types example](/Images/apim_yamles/yamles_types_example.png)

We can say that:

* As `JavaProcess.yaml` file is contained within `Process` directory, then `JavaProcess` is a child type of `Process` type.
* As `NetService.yaml` file is contained within `JavaProcess` directory, then `NetService` is a child type of `JavaProcess` type.

## Custom types

You can add custom types by creating a YAML file definition of an entity type, and placing the file in the correct subdirectory, under `META-INF/types`.

The following example shows how to create an entity type named `AnotherNetService`:

```yaml
name: AnotherNetService
version: 5
fields:
  anotherField:
    type: string
    defaultValues:
      - {}
    cardinality: 1
constants:
  executableImage:
    type: string
    value: vshell
components:
  LoadableModule: '?'
  ClassLoader: '?'
```

In the resulting directory structure, `AnotherNetService.yaml` is contained inside the `JavaProcess` directory, meaning that `AnotherNetService` type is a child type of the `JavaProcess` type.

![types example](/Images/apim_yamles/yamles_types_custom_example.png)

When adding a new custom entity type, the parent entity type of your custom type might not yet have any child entity types. In this case, create a new directory named the same as the parent type, without `.yaml` extension, at the same level as the parent type YAML file. Then, add your new custom entity type YAML file in the newly created directory.

YAML files for entity instances of custom types must be placed in the `System` directory. For more information, see [Directory Mapping](/docs/apim_yamles/apim_yamles_references/yamles_top_directories).

## Cardinality

The following table shows the meaning of cardinality symbol found in `types`:

| Symbol | Min | Max | Mandatory |
|:------:|:---:|:---:|:---------:|
|   1    |  1  |  1  |    Yes (if no default value) |
|   +    |  1  |  ∞  |    Yes (if no default value) |
|   ?    |  0  |  1  |    No     |
|   *    |  0  |  ∞  |    No     |

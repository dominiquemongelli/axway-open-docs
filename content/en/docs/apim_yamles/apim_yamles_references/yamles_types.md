{
"title": "Entity types in YAML configuration",
"linkTitle": "Entity types in YAML configuration",
"weight":"110",
"date": "2020-09-25",
"description": "Learn how entity types are described in a YAML configuration."
}

The `types` folder located under the `META-INF` directory of a YAML configuration contains the definition of all the entity types in the Entity Store model. An [entity type](/docs/apigtw_devguide/entity_store/#entity-types) is a description of an entity in the Entity Store.

The `types` folder is basically a directory tree on which relationships between types are managed in the logical manner of a directory tree.

The YAML Entity Store supports all entity types *and* custom types.

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

Considering the following types: `Process`, `JavaProcess` and `NetService`

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

And considering the following directory structure

![types example](/Images/apim_yamles/yamles_types_example.png)

We could say that:

* As *JavaProcess.yaml* file is containing within *Process* directory: `JavaProcess` is a child type of `Process` type.
* As *NetService.yaml* file is containing within *JavaProcess* directory: `NetService` is a child type of `JavaProcess` type.

## Custom types

Custom types can be added by providing a yaml file definition of an entity type, and put in the right directory.

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

![types example](/Images/apim_yamles/yamles_types_custom_example.png)

Looking at directory structure, *AnotherNetService.yaml* is containing inside *JavaProcess* directory. It means that `AnotherNetService` type is a child of `JavaProcess` type.

## Cardinality

| Symbol | Min | Max | Mandatory |
|:------:|:---:|:---:|:---------:|
|   1    |  1  |  1  |    Yes (if no default value) |
|   +    |  1  |  ∞  |    Yes (if no default value) |
|   ?    |  0  |  1  |    No     |
|   *    |  0  |  ∞  |    No     |

## Navigate in types directory

To create a `NetService` type

* Identify under which directory the *NetService.yaml* file should be put in.
* If the type you inherit from does not have any child, create a directory named as the type name (without *yaml* extension) aside of the parent type definition
* Put the *NetService.yaml* under the folder identified above. Make sure that the type name within the file matches the filename without *yaml* extension
* In order to know what fields can be used, move up the types hierarchy.
* add your own custom fields if you need to
* Search for components in the directory tree (note that some can be defined in the ancestor).
* `NetService` has two components.
    * Search for `"name: LoadableModule"` and/or for `"name: ClassLoader"`.
    * Do first steps again to get all required and optional fields for each entity type.

{{% alert color="warning" %}}Despite what is in the model, some fields are said to be mandatory (cardinality=1 and no default value) are not mandatory. Double check with Policy Studio if in doubt.{{% /alert %}}

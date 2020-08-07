# Smoothy.cloud - Application template API reference

An application is defined using a YAML specification in a `template.yml` file and optionally a `migrations.yml` file.

## Types

The marketplace API supports the following primitive types:

- string
- text
- boolean
- integer
- map
- null

In addition, it also support the following complex types:

- [Expression](#expression)
- [Boolean expression](#boolean-expression)
- [Atom](#atom)
- [Parameterized string](#parameterized-string)
- [Reference](#reference)

### Expression

The expression type resembles one of the following patterns:

- [atom](#atom)
- [atom](#atom) is [atom](#atom)
- [atom](#atom) is not [atom](#atom)
- {{ random_string(64) }}

### Boolean expression

The boolean expression type resembles one of the following patterns:

- boolean
- [atom](#atom) is [atom](#atom)
- [atom](#atom) is not [atom](#atom)

### Atom

The atom type resembles one of the following patterns:

- [parameterized string](#parameterized-string)
- boolean
- integer
- null

### Parameterized string

The parameters string type resembles a string optionally containing one or more [references](#reference).

### Reference

The reference type is a string with the following format:

{{ `type`.`name` }}

*Examples:*

- {{ variable.mysql_version }}
- {{ container.mysql }}
- {{ endpoint.mysql_internal_endpoint }}
- {{ volume.mysql_data }}

## The smoothy.yml file

The `smoothy.yml` file resembles an object with the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| api | string | yes | The version of the marketplace specification that is used in this file. |
| meta | [meta](#meta) | yes | Meta information about the application that is shown in the marketplace. |
| form | [form](#form) | no | The layout of the form that is used to gather the required input to install the application. |
| deployment | [deployment](#deployment) | yes | The resources that need to be deployed in order to install the application. |
| interface | [interface](#interface) | no | The parts of the application that are visible to or can be managed by the user through the Smoothy interface. |

### Meta

The meta property resembles an object with the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| icon | string | no |  |
| name | string | yes | The name of the application, which is shown within the Smoothy interface. |
| baseline | string | yes | A short 1-line description of the application. |
| description | text | yes | An extensive description of the application. |
| categories | list of strings | yes | The categories to which the application belongs. These are used to filter the templates within the Smooty interface.  |
| requirements | list of [requirements](#requirement) | no | A list of steps that need to be taken, if any, before the application can be installed. |

### Requirement

The requirement object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes |  |
| description | text | yes |  |

### Form

The form property resembles a list of [form steps](#form-step).

### Form step

The form step object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes |  |
| description | text | no |  |
| questions | list of [questions](#question) | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Question

The question object has the following properties:

| Property | Type | Required | Default | Description |
|---|---|---|---|---|
| variable | string (unique) | yes |  |  |
| label | string | yes |  |  |
| hint | text | no |  |  |
| required | [boolean expression](#boolean-expression) | no | false |  |
| default | map, [expression](#expression) | no |  |  |
| type | [question type](#question-type) | yes |  |  |
| options | list of [select options](#select-option) | if type is select |  |  |
| image_type | [parameterized string](#parameterized-string) | if type is image |  |  |
| immutable | [boolean expression](#boolean-expression) | no | false |  |
| if | [boolean expression](#boolean-expression) | no |  |  |

### Question type

The question type is one of the following strings:

- string
- text
- select
- boolean
- url
- integer
- password
- email
- image
- timezone
- map

### Select option

The select option object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| value | string | yes |  |
| label | string | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Deployment

The deployment property resembles a list of [resources](#resource).

### Resource

The resource object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| resource | [resource type](#resource-type) | yes |  |
| name | string (unique per resource type) | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

The following resource types have some additional properties:

- container:  [additional properties](#container)

### Resource type

The resource type is one of the following strings:

- volume
- container

### Container

The container object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| image | [parameterized string](#parameterized-string) | yes |  |
| endpoints | list of [endpoints](#endpoint) | no |  |
| volume_mounts | list of [volume mounts](#volume-mount) | no |  |
| environment | list of [environment variables](#environment-variable) and [environment variable maps](#environment-variable-map) | no |  |
| command | string, list of [command parts](#command-part) | no |  |

### Endpoint

The endpoint object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| name | string (unique) | yes |  |
| type | [endpoint type](#endpoint-type) | yes |  |
| port | integer | yes |  |
| domain | [parameterized string](#parameterized-string) | no |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Endpoint type

The endpoint type is one of the following strings:

- container_port
- host_port
- domain

### Volume mount

The volume mount object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| volume | [reference](#reference) to a [volume resource](#resource) | yes |  |
| mounth_path | [parameterized string](#parameterized-string) | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Environment variable

The environment variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| key | [parameterized string](#parameterized-string) | yes |  |
| value | [expression](#expression) | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Environment variable map

The environment variable map object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| map | [reference](#reference) to a [variable with type map](#question) | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Command part

The command part object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| part | [expression](#expression) | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Interface

The interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| endpoints | list of [endpoint interfaces](#endpoint-interface) | no |  |
| volumes | list of [volume interfaces](#volume-interface) | no |  |
| logs | list of [log interfaces](#log-interface) | no |  |

### Endpoint interface

The endpoint interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| show | list of [show endpoint interfaces](#show-endpoint-interface) | no |  |
| create | list of [create endpoint interfaces](#create-endpoint-interface) | no |  |

### Show endpoint interface

The show endpoint interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes |  |
| description | text | no |  |
| endpoint | [reference](#reference) to an [endpoint](#endpoint) | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Create endpoint interface

The create endpoint interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes |  |
| description | text | no |  |
| container | [reference](#reference) to a [container resource](#resource) | yes |  |
| type | [create endpoint type](#create-endpoint-type) | yes |  |
| port | integer | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Create endpoint type

The create endpoint type is one of the following strings:

- host_port
- domain

### Volume interface

The volume interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes | The title of the volume interface. <br><br> *Example:* <br> Uploaded documents |
| description | text | no | A short description explaining the user which data is contained in the volume. |
| volume | [reference](#reference) to a [volume resource](#resource) | yes | The volume that should be shown in the Smoothy interface. <br><br> *Example:* <br> {{ volume.mysql_data }} |
| if | [boolean expression](#boolean-expression) | no | Define dynamically if the volume interface should be shown. <br><br> *Example:* <br> {{ variable.mysql_version is "8.0" }} |

### Log interface

The log interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes | The title of the volume interface. <br><br> *Example:* <br> MySQL logs |
| description | text | no | A short description explaining the user which logs are shown. |
| container | [reference](#reference) to a [container resource](#resource) | yes | The container of which the logs should be shown in the Smoothy interface. <br><br> *Example:* <br> {{ container.mysql }} |
| if | [boolean expression](#boolean-expression) | no | Define dynamically if the log interface should be shown. <br><br> *Example:* <br> {{ variable.mysql_version is "8.0" }} |

## The migrations.yml file

The `migrations.yml` file is a multi-document YAML file in which each document contains one [migration](#migration).

### Migration

The migration object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| api | string | yes | The version of the template API that is used in this migration. <br><br> *Example:* <br> v1 |
| from | string | yes | The semantic version of the application before the migration. <br><br> *Example:* <br> 1.0.0 |
| to | string | yes | The semantic version of the application after the migration. <br><br> *Example:* <br> 1.0.1 |
| changes | list of [changes](#change) | yes | The changes that need to be applied in order to migrate the application. |

### Change

The change object has *one* of the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| if | [boolean expression](#boolean-expression) | no |  |

In addition, it has *one* of the following properties:

| Property | Type | Description |
|---|---|---|
| add variable | [add variable](#add-variable) | Add a new input variable to the application configuration. |
| rename variable | [rename variable](#rename-variable) | Rename a variable in the application configuration. |
| remove variable | [remove variable](#remove-variable) | Remove a variable from the application configuration. |
| replace variable | [replace variable](#replace-variable) | Replace the value of a variable in the application configuration. |
| update endpoint | [update endpoint](#update-endpoint) | Update all the existing endpoints of a given type. |
| remove endpoint | [remove endpoint](#remove-endpoint) | Remove all the existing endpoints of a given type. |

### Add variable

The add variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| variable | string | yes | The name of the variable that is added. |
| value | [expression](#expression) | yes | The value of the variable that is added. |

### Rename variable

The rename variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| old | string | yes | The old name of the variable that is renamed. |
| new | string | yes | The new name of the variable that is renamed. |

### Remove variable

The remove variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| variable | string | yes | The name of the variable that is removed. |

### Replace variable

The replace variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| variable | string | yes | The name of the variable of which the value is replaced. |
| switch | list of [replacement cases](#replacement-case) | no |  |
| default | [expression](#expression) | no |  |

### Replacement case

The replacement object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| case | [expression](#expression) | yes | The old value of the variable. |
| value | [expression](#expression) | yes | The new value of the variable. |
| if | [boolean expression](#boolean-expression) | no |  |

### Update endpoint

The update endpoint object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| container | [reference](#reference) to a [container resource](#resource) | yes |  |
| type | [create endpoint type](#create-endpoint-type) | yes |  |
| port | integer | yes |  |
| new_container | [reference](#reference) to a [container resource](#resource) | no |  |
| new_type | [create endpoint type](#create-endpoint-type) | no |  |
| new_port | integer | no |  |
| if | [boolean expression](#boolean-expression) | no |  |

### Remove endpoint

The remove endpoint object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| container | [reference](#reference) to a [container resource](#resource) | yes |  |
| type | [create endpoint type](#create-endpoint-type) | yes |  |
| port | integer | yes |  |
| if | [boolean expression](#boolean-expression) | no |  |
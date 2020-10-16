# The template.yml file

The `template.yml` file resembles an object with the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| api | string | yes | The version of the marketplace specification that is used in this file. |
| meta | [meta](#meta) | yes | Meta information about the application that is shown in the marketplace. |
| form | [form](#form) | no | The layout of the form that is used to gather the required input to install the application. |
| deployment | [deployment](#deployment) | yes | The resources that need to be deployed in order to install the application. |
| interface | [interface](#interface) | no | The parts of the application that are visible to or can be managed by the user through the Smoothy interface. |

## Meta

The meta property resembles an object with the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| icon | string | no |  |
| name | string | yes | The name of the application, which is shown within the Smoothy interface. |
| baseline | string | yes | A short 1-line description of the application. |
| description | text | yes | An extensive description of the application. |
| categories | list of strings | yes | The categories to which the application belongs. These are used to filter the templates within the Smooty interface.  |
| requirements | list of [requirements](#requirement) | no | A list of steps that need to be taken, if any, before the application can be installed. |

## Requirement

The requirement object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes |  |
| description | text | yes |  |

## Form

The form property resembles a list of [form steps](#form-step).

## Form step

The form step object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes |  |
| description | text | no |  |
| questions | list of [questions](#question) | yes |  |
| if | [boolean expression](./types.md#expression) | no |  |

## Question

The question object has the following properties:

| Property | Type | Required | Default | Description |
|---|---|---|---|---|
| variable | string (unique) | yes |  |  |
| label | string | yes |  |  |
| hint | text | no |  |  |
| required | boolean, [boolean expression](./types.md#expression) | no | false |  |
| default | [primitive type](./types.md#Types), [expression](./types.md#expression) | no |  |  |
| type | [question type](#question-type) | yes |  |  |
| options | list of [select options](#select-option) | if type is select |  |  |
| minimum | integer, number, [binary number](./types.md#binary-number) (depending on type) | no |  |  |
| maximum | integer, number, [binary number](./types.md#binary-number) (depending on type) | no |  |  |
| immutable | boolean, [boolean expression](./types.md#expression) | no | false |  |
| if | [boolean expression](./types.md#expression) | no |  |  |

## Question type

The question type is one of the following strings:

- string
- text
- select
- boolean
- url
- integer
- number
- binary_number
- password
- email
- code_repository
- timezone
- code
- map
- list

## Select option

The select option object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| value | string | yes |  |
| label | string | yes |  |
| if | [boolean expression](./types.md#expression) | no |  |

## Deployment

The deployment property resembles a list of [resources](#resource).

## Resource

The resource object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| resource | [resource type](#resource-type) | yes |  |
| name | string (unique per resource type) | yes |  |
| if | [boolean expression](./types.md#expression) | no |  |
| loop | list, [expression](./types.md#expression) | no | For each element in the provided list, one resource will be added. The value of the element can be accessed via the following expression `{{ loop.value }}`. The index of the item can be accessed via `{{ loop.key }}`. |

The following resource types have some additional properties:

- image: [additional properties](#image)
- config_file: [additional properties](#config-file)
- container: [additional properties](#container)
- service: [additional properties](#service)
- endpoint: [additional properties](#endpoint)

## Resource type

The resource type is one of the following strings:

- image
- volume
- config_file
- container
- service

## Image

The image has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| code_repository | [code_repository](#question-type), [expression](./types.md#expression)  | yes |  |
| dockerfile | string, [switch](./types.md#switch) | yes |  |
| arguments | list of [environment variables](#environment-variable) | no |  |

## Config file

The config file object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| contents | text, [switch](./types.md#switch) | yes |  |

## Container

The container object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| image | string, [expression](./types.md#expression), [reference](./types.md#reference) to an [Image resource](#resource), [switch](./types.md#switch) | yes |  |
| image_registry | [reference](./types.md#reference) to a [Docker Registry](#docker-registry), [switch](./types.md#switch) | no |  |
| volume_mounts | list of [volume mounts](#volume-mount) | no |  |
| config_file_mounts | list of [config file mounts](#config-file-mount) | no |  |
| environment | list of [environment variables](#environment-variable) and [environment variable maps](#environment-variable-map) | no |  |
| command | string, list of [command parts](#command-part) | no |  |
| memory | integer, [expression](./types.md#expression), [switch](./types.md#switch) | yes |  |
| cpus | integer, [expression](./types.md#expression), [switch](./types.md#switch) | yes |  |

## Docker Registry

Docker Registries can be added on your team's Settings page in Smoothy. You can reference a Docker Registry as follows:

`{* docker_registry.ID *}`

## Config file mount

The config file mount object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| config_file | [reference](./types.md#reference) to a [config file resource](#resource) | yes |  |
| mounth_path | string, [expression](./types.md#expression) | yes |  |
| if | [boolean expression](./types.md#expression) | no |  |

## Volume mount

The volume mount object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| volume | [reference](./types.md#reference) to a [volume resource](#resource) | yes |  |
| mounth_path | string, [expression](./types.md#expression) | yes |  |
| if | [boolean expression](./types.md#expression) | no |  |

## Environment variable

The environment variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| key | string, [expression](./types.md#expression) | yes |  |
| value | [primitive type](./types.md#Types), [expression](./types.md#expression) | yes |  |
| if | [boolean expression](./types.md#expression) | no |  |

## Environment variable map

The environment variable map object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| map | map, [expression](./types.md#expression) | yes |  |
| if | [boolean expression](./types.md#expression) | no |  |

## Command part

The command part object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| part | string, [expression](./types.md#expression) | yes |  |
| if | [boolean expression](./types.md#expression) | no |  |

## Service

The service object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes |  |
| description | text | no |  |
| container | [reference](./types.md#reference) to a [container resource](#resource) | yes |  |
| port | integer, [switch](./types.md#switch) | yes |  |

## Interface

The interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| volumes | list of [volume interfaces](#volume-interface) | no |  |
| logs | list of [log interfaces](#log-interface) | no |  |

## Volume interface

The volume interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes | The title of the volume interface. <br><br> *Example:* <br> Uploaded documents |
| description | text | no | A short description explaining the user which data is contained in the volume. |
| volume | [reference](./types.md#reference) to a [volume resource](#resource) | yes | The volume that should be shown in the Smoothy interface. <br><br> *Example:* <br> `{* volume.mysql_data *}` |
| if | [boolean expression](./types.md#expression) | no | Define dynamically if the volume interface should be shown. <br><br> *Example:* <br> {{ variable.mysql_version is "8.0" }} |
| loop | list, [expression](./types.md#expression) | no | For each element in the provided list, one volume interface will be added. The value of the element can be accessed via the following expression `{{ loop.value }}`. The index of the item can be accessed via `{{ loop.key }}`. |

## Log interface

The log interface object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes | The title of the volume interface. <br><br> *Example:* <br> MySQL logs |
| description | text | no | A short description explaining the user which logs are shown. |
| container | [reference](./types.md#reference) to a [container resource](#resource) | yes | The container of which the logs should be shown in the Smoothy interface. <br><br> *Example:* <br> `{* container.mysql *}` |
| if | [boolean expression](./types.md#expression) | no | Define dynamically if the log interface should be shown. <br><br> *Example:* <br> {{ variable.mysql_version is "8.0" }} |
| loop | list, [expression](./types.md#expression) | no | For each element in the provided list, one log interface will be added. The value of the element can be accessed via the following expression `{{ loop.value }}`. The index of the item can be accessed via `{{ loop.key }}`. |
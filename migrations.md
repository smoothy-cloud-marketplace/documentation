# The migrations.yml file

The `migrations.yml` file is a multi-document YAML file in which each document contains one [migration](#migration).

## Migration

The migration object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| api | string | yes | The version of the template API that is used in this migration. <br><br> *Example:* <br> v1 |
| from | string | yes | The semantic version of the application before the migration. <br><br> *Example:* <br> 1.0.0 |
| to | string | yes | The semantic version of the application after the migration. <br><br> *Example:* <br> 1.0.1 |
| changes | list of [changes](#change) | yes | The changes that need to be applied in order to migrate the application. |

## Change

The change object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| if | [boolean expression](./types.md#boolean-expression) | no |  |

In addition, it also has *one* of the following properties:

| Property | Type | Description |
|---|---|---|
| add variable | [add variable](#add-variable) | Add a new input variable to the application configuration. |
| rename variable | [rename variable](#rename-variable) | Rename a variable in the application configuration. |
| remove variable | [remove variable](#remove-variable) | Remove a variable from the application configuration. |
| replace variable | [replace variable](#replace-variable) | Replace the value of a variable in the application configuration. |
| update endpoint | [update endpoint](#update-endpoint) | Update all the endpoints of a given type. |
| remove endpoint | [remove endpoint](#remove-endpoint) | Remove all the endpoints of a given type. |

## Add variable

The add variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| variable | string | yes | The name of the variable that is added. |
| value | [expression](./types.md#expression) | yes | The value of the variable that is added. |

## Rename variable

The rename variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| old | string | yes | The old name of the variable that is renamed. |
| new | string | yes | The new name of the variable that is renamed. |

## Remove variable

The remove variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| variable | string | yes | The name of the variable that is removed. |

## Replace variable

The replace variable object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| variable | string | yes | The name of the variable of which the value is replaced. |
| switch | list of [replacement cases](#replacement-case) | no |  |
| default | [expression](./types.md#expression) | no |  |

## Replacement case

The replacement object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| case | [expression](./types.md#expression) | yes | The old value of the variable. |
| value | [expression](./types.md#expression) | yes | The new value of the variable. |
| if | [boolean expression](./types.md#boolean-expression) | no |  |

## Update endpoint

The update endpoint object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| container | [reference](./types.md#reference) to a [container resource](./template.md#resource) | yes |  |
| type | [create endpoint type](./template.md#create-endpoint-type) | yes |  |
| port | integer | yes |  |
| new_container | [reference](./types.md#reference) to a [container resource](./template.md#resource) | no |  |
| new_type | [create endpoint type](./template.md#create-endpoint-type) | no |  |
| new_port | integer | no |  |
| if | [boolean expression](./types.md#boolean-expression) | no |  |

## Remove endpoint

The remove endpoint object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| container | [reference](./types.md#reference) to a [container resource](./template.md#resource) | yes |  |
| type | [create endpoint type](./template.md#create-endpoint-type) | yes |  |
| port | integer | yes |  |
| if | [boolean expression](./types.md#boolean-expression) | no |  |
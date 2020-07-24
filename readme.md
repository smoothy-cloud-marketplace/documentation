# Smoothy.cloud marketplace - API reference

An application is defined using a YAML specification in a `smoothy.yml` file.

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

...

### Reference

...

## The smoothy.yml file

The `smoothy.yml` file resembles an object with the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| api | string | yes | The version of the marketplace specification that is used in this file. |
| meta | [meta](#meta) | yes | Meta information about the application that is shown in the marketplace. |
| form | [form](#form) | yes | The layout of the form that is used to gather the required input to install the application. |
| deployment | [deployment](#deployment) | yes | The resources that need to be deployed in order to install the application. |
| interface | [interface](#interface) | yes | The parts of the application that are visible to or can be managed by the user through the Smoothy interface. |

### Meta

The meta property resembles an object with the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| icon | string | no |  |
| name | string | yes |  |
| baseline | string | yes |  |
| description | text | yes |  |
| categories | list of strings | yes |  |
| requirements | list of [requirements](#requirement) | no |  |

### Requirement

The requirement object has the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| title | string | yes |  |
| description | text | yes |  |

### Form

The form property resembles a list of [form steps](#form-steps).

### Form step

The form group object has the following properties:

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
| default | map or [expression](#expression) | no |  |  |
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

...

### Interface

...

## The migrations.yml file

...
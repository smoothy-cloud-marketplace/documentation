# Types

The marketplace API supports the following primitive types:

- string
- text
- boolean
- integer
- number
- map
- null

In addition, it also support the following complex types:

- [Binary-number](#binary-number)
- [Expression](#expression)
- [Reference](#reference)
- [Switch](#switch)

## Binary number

The binary number type resembles a number to the power of two. Examples:

- 64 (= 2^6)
- 128 (= 2^7)
- 256 (= 2^8)
- 512 (= 2^9)
- 1024 (= 2^10)
- 2048 (= 2^11)
- 4096 (= 2^12)
- 8192 (= 2^13)
- ...

## Expression

An expression is a string that is interpreted and resolved upon evaluation. As shown in the table below, it can contain mathematical operations, comparisons, string manipulations, etc. It can also contain references to variables which get replaced with their respective values when the template is evaluated.

The table below illustrates the possibilities of expressions. In the examples, the following list of variables is used:

- name: "John"
- price: 20

| Expression | Result |
|---|---|
| *string* |  |
| `"Hello"` | `"Hello"` |
| `"Hello {{ variable.name }}"` | `"Hello John"` |
| `"{{ variable.name }}"` | `"John"` |
| `"{* container.{{ 'database' }} *}"` | `"{* container.database *}"` |
| `"123"` | `"123"` |
| `"8.0"` | `"8.0"` |
| *boolean* |  |
| `true` | `true` |
| `false` | `false` |
| *numbers* |  |
| `123` | `123` |
| `123.45` | `123.45` |
| `"{{ 123 }}"` | `123` |
| `"{{ 123.45 }}"` | `123.45` |
| *math* |  |
| `"{{ 1 + 1 + 1 }}"` | `3` |
| `"{{ 1 + 1 - 1 }}"` | `1` |
| `"{{ 9 / 3 - 1 }}"` | `2` |
| `"{{ (4 / 3)\|round + 4 }}"` | `5` |
| `"1{{ 1 + 1 }}3"` | `"123"` |
| `"price: {{ variable.price * 0.8 }}.99"` | `"price: 16.99"` |
| *comparisons* |  |
| `"{{ 1 == 1 }}"` | `true` |
| `"{{ 1 != 1 }}"` | `false` |
| `"{{ 2 < variable.price }}"` | `true` |
| `"{{ 2 >= 2 }}"` | `true` |
| `"{{ 2 is odd }}"` | `false` |
| `"{{ variable.price is even }}"` | `true` |
| `"{{ variable.name == 'Jane' }}"` | `false` |
| `"{{ variable.place == null }}"` | `true` |
| `"{{ variable.price in [16,18,20] }}"` | `true` |
| `"{{ variable.price not in [16,18,20] }}"` | `false` |
| *logic* |  |
| `"{{ variable.price in [16,18,20] and 1 > 2 }}"` | `false` |
| `"{{ variable.price in [16,18,20] or 1 > 2 }}"` | `true` |
| *regular expressions* |  |
| `"{{ '1.0.1' matches '/^[0-9]+\.[0-9]+\.[0-9]+$/' }}"` | `true` |
| `"{{ variable.name matches '/^J.+$/' }}"` | `true` |
| *random string* |  |
| `"{{ str_random() }}"` | `"M2rbzUqcBKepscWO"` |
| `"{{ str_random(8) }}"` | `"r7PA4fxs"` |
| *filters* |  |
| `"Hello {{ variable.name\|upper }}"` | `"Hello JOHN"` |
| `"Hello {{ variable.name\|upper }}"` | `"Hello JOHN"` |
| `"Hello {{ 'john'\|capitalize }}"` | `"Hello John"` |
| `"Hello {{ variable.name\|default('Jane') }}"` | `"Hello John"` |
| `"Hello {{ variable.first_name\|default('Jane') }}"` | `"Hello Jane"` |

## Reference

The reference type is a string with the following format:

{* `type`.`name` *}

*Examples:*

- `{* container.mysql *}`
- `{* endpoint.mysql_internal_endpoint *}`
- `{* volume.mysql_data *}`

## Switch

The switch type is an object with the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| switch | list of [cases](#switch-case) | yes |  |
| default | [primitive type](#Types), [expression](#expression) | no |  |

## Switch case

The switch case type is an object with the following properties:

| Property | Type | Required | Description |
|---|---|---|---|
| case | [boolean expression](#expression) | yes |  |
| value | [primitive type](#Types), [expression](#expression) | yes |  |
| if | [boolean expression](#expression) | no |  |

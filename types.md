# Types

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

## Expression

The expression type resembles one of the following patterns:

- [atom](#atom)
- [atom](#atom) is [atom](#atom)
- [atom](#atom) is not [atom](#atom)
- {{ random_string(64) }}

## Boolean expression

The boolean expression type resembles one of the following patterns:

- boolean
- [atom](#atom) is [atom](#atom)
- [atom](#atom) is not [atom](#atom)

## Atom

The atom type resembles one of the following patterns:

- [parameterized string](#parameterized-string)
- boolean
- integer
- null

## Parameterized string

The parameters string type resembles a string optionally containing one or more [references](#reference).

## Reference

The reference type is a string with the following format:

{{ `type`.`name` }}

*Examples:*

- {{ variable.mysql_version }}
- {{ container.mysql }}
- {{ endpoint.mysql_internal_endpoint }}
- {{ volume.mysql_data }}


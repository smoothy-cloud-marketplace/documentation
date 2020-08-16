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
- [Boolean expression](#boolean-expression)
- [Atom](#atom)
- [Parameterized string](#parameterized-string)
- [Reference](#reference)

# Binary number

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


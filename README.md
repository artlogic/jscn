JSCN
====

JSCN (JavaScript Configuration Notation) is a human readable dialect of [JSON](http://json.org/) designed with config files in mind, inspired by [TOML](https://github.com/mojombo/toml).

Goals/Requirements
------------------

* Easier to read/write than standard JSON.
* Comments!
* Multi-line strings.
* The dialect should be forgiving enough to parse any JSON document with an object as a root element.
* Ability to deal with more data types at a later date.

Changes from JSON
-----------------

JSCN is almost JSON with some minor changes/extensions.  A simple JSON document will be transformed into JSCN to illustrate:

```json
{
    "string": "this is a string",

    "array": [
        "an array",
        4,
        "you"
    ],

    "multi": "contrived\nexample\nstring"

    "object": {
        "number": 10,
        "boolean": true
    },
}
```

### Commas are optional in arrays and objects.

Commas are considered whitespace by JSCN.  Tokens in objects and arrays are separated by whitespace as opposed to commas.

```
{
    "string": "this is a string"

    "array": [
        "an array"
        4
        "you"
    ]

    "multi": "contrived\nexample\nstring"

    "object": {
        "number": 10
        "boolean": true
    }
}
```

### Quoting of keys in objects is optional.

In cases where ambiguity would occur (e.g. keys containing colons), keys should still be quoted.

```
{
    string: "this is a string"

    array: [
        "an array"
        4
        "you"
    ]

    multi: "contrived\nexample\nstring"

    object: {
        number: 10
        boolean: true
    }
}
```

### Standard single-line hash comments are allowed.

Comments denoted by the hash symbol, can either appear on a line by itself, or at the end of any line.  Either a line feed or carriage return (as defined by the [JSON RFC](http://www.ietf.org/rfc/rfc4627.txt)) ends the comment.

```
{
    # this is a comment
    string: "this is a string"

    array: [
        "an array"
        4
        "you"
    ]

    multi: "contrived\nexample\nstring"

    object: {
        number: 10
        boolean: true  # another comment
    }
}
```

### Strings can be specified with heredoc syntax.

JSCN uses ruby style [heredoc](http://en.wikipedia.org/wiki/Here_document#Ruby) syntax: ```<<DELIM```.  The alternate ```<<-DELIM``` syntax is not supported.

```
{
    # this is a comment
    string: "this is a string"

    array: [
        "an array"
        4
        "you"
    ]

    multi: <<END
contrived
example
string
END

    object: {
        number: 10
        boolean: true  # another comment
    }
}
```

### The top-level value must be an object.  Its braces are optional.

The top-level element in a JSON document is either a string or an array, according to the RFC.  For most configuration files, only object is useful, so, in JSCN, only object is allowed.  Furthermore, the braces are optional on the top-level object.

```
# this is a comment
string: "this is a string"

array: [
    "an array"
    4
    "you"
]

multi: <<END
contrived
example
string
END

object: {
    number: 10
    boolean: true  # another comment
}
```

### Top level keys can be assigned to an object other than the root object by specifying a scope change by wrapping the new scope in square brackets on a line by itself: ```[settings]```.  You can access deeper scopes in the same way you would index objects in JavaScript: ```[settings][user][colors]```.

Example
-------

```
# This is a comment

key: "value"  # value on the root object

# Let's change the scope
[settings]
key: "value"  # settings.key

# the settings scope is in effect until another scope change

# How about a deeply nested scope?
[settings][user][colors]
text: "white"  # settings.user.colors.text
highlight: "blue"

# scope changes exist on a line by themselves
[example]
list1: [1, true, "string"]  # commas are optional
list2: [
  "item1"  # comments can be anywhere whitespace is valid
  "item2"
]

template: <<END
This is a heredoc string.  It can contain
 * newlines
 * quotes "
 * backslashes \
 * whatever...
END

# leading whitespace is inconsequential
    key: "value"

# keys can be quoted, optionally
"another key": "value"

# but don't generally need to be (unless they contain a square bracket or a colon)
$a crazy key!: "value"

# scopes can also be quoted (and empty!)
["new scope"]

# but don't generally need to be (unless they contain a square bracket)
[$crazy scope!]

# It's unlikely you would use this syntax, but for completeness
object1: {key1: "value1", key2: "value2"}  # commas are optional
object2: {
  key1: "value1"
  key2: "value2"
}
```

Data Type Extensions
--------------------

At some point in the future, more data types may be added.  I imagine these will be represented in JSON with MongoDB's [Extended JSON](http://docs.mongodb.org/manual/reference/mongodb-extended-json/).  In a JSCN file, they will have a much simpler representation.

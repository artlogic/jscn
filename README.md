JSCN
====

JSCN (JavaScript Configuration Notation) is a human readable dialect of JSON designed with config files in mind.  Inspired by [TOML](https://github.com/mojombo/toml).  JSCN are meant to be compiled into JSON that can be parsed by any standard parser.

Goals/Requirements
------------------

* Easier to read/write than standard JSON.
* Comments!
* Multi-line strings.
* The dialect should be forgiving enough to parse any JSON document with an object as a root element.
* Ability to deal with more data types at a later date.

Changes from JSON
-----------------

JSCN is almost JSON with some minor changes/extensions.

### The top-level value must be an object.  Its braces are optional.

The top-level element in a JSON document is one of the following:

* string
* number
* object
* array
* true
* false
* null

For configuration files, only object is really useful, so, in JSCN, only object is allowed.  Furthermore, the braces are optional on the top-level object.  In JSON, you would write this:

```json
{
  "property": null
}
```

JSCN allows you to drop the braces for the top-level object only:

```
"property": null
```

### Commas are optional in arrays and objects.

In JSON, commas are needed to entries in arrays and objects.  In JSCN, they are optional:

```
["string" 42 true false null]

{"property1": true "property2": 10}
```

* Strings can be specified with ruby style [heredoc](http://en.wikipedia.org/wiki/Here_document#Ruby) syntax: ```<<DELIM```
* Comments can either appear on a line by themselves, or at the end of a line.  They are denoted by a hash symbol.
* Quoting of keys in objects is optional, except in cases where the key name might cause parsing ambiguity.
* Top level keys can be assigned to an object other than the root object by specifying a scope change by wrapping the new scope in square brackets on a line by itself: ```[settings]```.  You can access deeper scopes in the same way you would index objects in JavaScript: ```[settings][user][colors]```.

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

Compiled JSON
-------------

Forthcoming.

Why?
----

I really like JSON, but it doesn't have comments or multiline/raw strings (and can be error-prone to write).  It could be nicer to read.

Data Type Extensions
--------------------

At some point in the future, more data types may be added.  I imagine these will be represented in JSON with MongoDB's [Extended JSON](http://docs.mongodb.org/manual/reference/mongodb-extended-json/).  In a JSCN file, they will have a much simpler representation.

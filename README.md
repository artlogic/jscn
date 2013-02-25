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

* The only valid root value in a JSCN document is an object.  The braces on the root object are optional.
* Commas are optional in arrays and objects (and are ignored as whitespace).
* Strings can be specified with ruby style [heredoc](http://en.wikipedia.org/wiki/Here_document#Ruby) syntax: ```<<DELIM```
* Comments can either appear on a line by themselves, or at the end of a line.  They are denoted by a hash symbol.
* Quoting of keys in objects is optional, except in cases where the key name might cause parsing ambiguity.
* Top level keys can be assigned to an object other than the root object by specifying a scope change by wrapping the new scope in square brackets on a line by itself: ```[settings]```.  You can access deeper scopes in the same way you would index objects in JavaScript: ```[settings][user][colors]```.

Example
-------

```
# This is a comment

key: "value"  # value on the root object

list1: [1 true "string"]
list2: [
  "item1"  # comments can be anywhere whitespace is valid
  "item2"
]

object1: {
  key1: "value1"
  key2: "value2"
}

template: <<END
This is a heredoc string.  It can contain
 * newlines
 * quotes "
 * backslashes \
 * whatever...
END

# Let's change the scope
[settings]
key: "value"  # settings.key

# How about a deeply nested scope?
[settings][user][colors]
text: "white"  # settings.user.colors.text
highlight: "blue"
```

Why?
----

I really like JSON, but it doesn't have comments or multiline/raw strings (and can be error-prone to write).  It could be nicer to read.

Data Type Extensions
--------------------

At some point in the future, more data types may be added.  I imagine these will be represented in JSON with MongoDB's [Extended JSON](http://docs.mongodb.org/manual/reference/mongodb-extended-json/).  In a JSCN file, they will have a much simpler representation.

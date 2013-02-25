JSCN
====

JSCN (JavaScript Configuration Notation) is a human readable dialect of JSON designed with config files in mind.  Inspired by [TOML](https://github.com/mojombo/toml).  JSCN are meant to be compiled into JSON that can be parsed by any standard parser.

Goals/Requirements
------------------

* Easier to read/write than standard JSON.
* Comments!
* Multi-line strings.
* The dialect should be forgiving enough to parse any JSON document with an object as a root element.
* Ability to deal with more data types later on.

Changes from JSON
-----------------

* The only valid root value in a JSCN document is an object.  The braces on the root object are optional.
* Commas are optional in arrays and objects (and are ignored as whitespace).
* Strings can be specified with ruby style [heredoc](http://en.wikipedia.org/wiki/Here_document#Ruby) syntax: ```<<DELIM```
* Comments can either appear on a line by themselves, or at the end of a line.  They are denoted by a hash symbol.
* Quoting of keys in objects is optional, except in cases where the key name might cause parsing ambiguity.
* Object headers - forthcoming.

Example
-------

Forthcoming.

Why?
----

I really like JSON, but it doesn't have comments or multiline/raw strings (and can be error-prone to write).  On top of that, it's not much fun to read.

Data Type Extensions
--------------------

At some point in the future, more data types may be added.  I imagine these will be represented in JSON with MongoDB's [Extended JSON](http://docs.mongodb.org/manual/reference/mongodb-extended-json/).  In a JSCN file, they will have a much simpler representation.

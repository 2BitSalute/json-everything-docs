---
layout: page
title: JsonPath.Net Basics
md_title: _JsonPath.Net_ Basics
bookmark: JSON Path
permalink: /path/:title/
icon: fas fa-tag
order: "2.1"
---
JSON Path is a query language for JSON documents inspired by what XPath provides for XML documents.  It was [originally proposed](https://goessner.net/articles/JsonPath/) by Matt Goëssner, and now a [specification](https://github.com/jsonpath-standard/internet-draft) is in progress.

Version 0.5.x is aligned with the specification as of the end of April 2023, plus additional support for the following features, which were not included in the specification:

- arithmetic operations in expressions
- `in` operator in expressions
- starting a path with `@` instead of `$` (relative paths)
- object and arrays in expressions (does not support single quotes)

## Syntax {#path-syntax}

A path consists of start indicator `$` followed by a series of segments, chained one after another.  Each segment contains one or more selectors.  Each selector takes in a collection of JSON nodes and produces a collection of JSON nodes (called a "nodelist") based on their function.  The output of the segment is the collective output of all of that segment's selectors.  Each segment takes as input the output of the previous selector.

- `$` - This is the root.  A path must always begin with it.  It effectively "selects" the root document.  It can also be used in query expressions, which we'll come to later.
- `[...]` - Square brackets indicate a segment.  Selectors are found between the brackets, separated by commas.  There are several kinds of selectors:
  - Index - The basic integer index that we're all familiar with.  Negative numbers will start counting from the end of an array.  If the value is out of the range of the array, no nodes will be returned.
  - [Slice](https://stackoverflow.com/a/509295/878701) - This allows selection of a range of nodes from an array.  Again, this range is clamped to the bounds of the array.
  - Property name - This allows property selection of objects by matching the full name.  Names can be single- or double-quoted and special characters must be encoded as is required by JSON.
  - Wildcard (`*` literal) - This simply returns all values from within an object or array.
  - Filter - This is an expression that evaluates to a boolean and operates on the child nodes of the node being passed to the selector.  It is denoted by a question mark followed an expression `?...` that returns a boolean result. (see note below)
- `..` - This is a recursive descent operator.  It is not itself a segment, but a segment prepended with this operator will recursively query the entire subtree rather than just the local value.

> This boolean result is distinct from a JSON boolean, which is denoted by either the `true` or `false` literals.  Using `true` or `false` in an expression is interpreted as a JSON literal and must be used in a comparison.
{: .prompt-info }

In addition to the above, there are a few shorthand options for some special cases.   These are only valid when the segment contains only a single selector and that selector is either a property name or a wildcard.

- `['foo']` may be rewritten as `.foo`.
- `[*]` may be rewritten as `.*`.
- `..['foo']` may be rewritten as `..foo`
- `..[*]` may be rewritten as `..*`

### Query Expressions {#path-expressions}

Filter selectors take an expression.  This expression uses a single variable, which is a JSON node as described above.  The node is denoted by `@` and any valid JSON Path can follow it.  The `@` is a stand-in for the `$` from above and acts as the root of the local value.

For example, an item query expression may be `?@.price<10`.  This expression will find all items in either an object or an array that contains a `price` property with a value less than 10.

Additionally, the `$` selector may also be used to reference back to the root node.  This allows queries like `?@.price<$.maxPrice` where we want to find all of the items of the current container that contain a `price` value that is less than the value in the root node's `maxPrice` property.

> This library considers paths that start with `$` to be "globally scoped" and paths that start with `@` to be "locally scoped."
{: .prompt-tip }

Expressions support the following operations:

- Arithmetic
  - `+`
  - `-`
  - `*`
  - `/`
  <!-- - `%` (modulus) -->
- Comparison
  - `==`
  - `!=`
  - `<`
  - `<=`
  - `>`
  - `>=`
- Boolean logic
  - `&&`
  - `||`

> Arithmetic operations are not part of the specification (yet), and cannot be expected to work in other JSON Path implementations.
{: .prompt-info }

#### Functions {#path-functions}

There is also support for functions within query expressions, which works as an extension point to add your own custom logic.

A function is a name followed by a parentheses containing zero or more parameters separated by commas.  Parameters can be JSON literals, JSON Paths (global or local), or even other functions.  (That is, the return value of function calls can be parameters, e.g. `min(max(@,0),10)`; passing one function into another isn't supported.)

The specification defines the following functions:

- `length(<value>)` to return the length of a string or the number of items in an array or object.  Takes a single parameter.
- `count(<nodelist>)` to return the number of nodes in a nodelist.
- `match(<text>, <iregexp>)` to return whether the text is an exact match for a regular expression per [I-Regexp](https://www.ietf.org/archive/id/draft-ietf-jsonpath-iregexp-02.html). (see note below)
- `search(<text>, <iregexp>)` to return whether the text _contains_ an exact match for a regular expression per [I-Regexp](https://www.ietf.org/archive/id/draft-ietf-jsonpath-iregexp-02.html). (see note below)
- `value(<nodelist>)` to convert a nodelist to a value: single-nodelists extract their node's value; multiple-/empty nodelists convert to `Nothing` (no node, absent).

> I-Regexp is designed to be an interoperable subset of most popular regular expression specifications and implementations.  One difference that [could not be resolved](https://github.com/ietf-wg-jsonpath/iregexp/issues/15) was implicit anchoring.  As such, two methods were developed to handle both cases.  `match` uses implicit anchoring, while `search` does not.
{: .prompt-info }

#### Custom Functions {#path-custom-functions}

There is also support for defining your own functions.

To do this, you'll need to decide what type your function will return and derive from the appropriate base class:

| Return type | What that means | Base class | Associated C# type |
|:-|:-|:-|:-|
| `ValueType` | JSON values or `Nothing` | `ValueFunctionDefinition` | `JsonNode?` |
| `LogicalType` | a boolean (not related to the JSON `true`/`false` literals) | `LiteralFunctionDefinition` | `bool` |
| `NodesType` | a nodelist, as returned by `@.a` or `@.*` | `NodeListFunctionDefinition` | `NodeList` |

Once you've created your function class, you'll need to implement the `Name` property (abstracted in the base class) to identify the function.  You'll also need to create an `Evaluate()` function that

- returns the associated C# type for the base class
- only has parameters of one of the above types

You're welcome to have as many parameters as you want, as long as they're one of the three types listed in the table.

For an example, please see the [`LengthFunction` implementation](https://github.com/gregsdennis/json-everything/blob/master/JsonPath/LengthFunction.cs) in the code.

Now that your function class is created, all that's left is to register it:

```c#
FunctionRepository.Register(new MyCustomFunc());
```

## In Code {#path-in-code}

To obtain an instance of a JSON Path, you'll need to parse it from a string.

```c#
var path = JsonPath.Parse("$.prop[0:6:2]");
```

or

```c#
var success = JsonPath.TryParse("$.prop[0:6:2]", out JsonPath path);
```

This will create a `JsonPath` instance that will select the `prop` property of an object, and subsequently items, 0, 2, and 4 of an array that resides there.

Once your path is created, you can start evaluating instances.

```c#
var instance = JsonNode.Parse("{\"prop\":[0,1,2,3]}");

var results = path.Evaluate(instance);
```

This will return a results object that contains the resulting nodelist or an error.

A node contains both the value that was found and the location in the instance _where_ it was found.  The location is always represented using the "canonical," bracketed format.

## Adherence to the Proposed Specification {#path-spec}

As the specification is still under authorship, there are features present in traditional JSON Path that haven't been properly described yet.  For these features, this library has been configured to mimic the consensus behaviors of other libraries as determined by the [JSON Path Comparison](https://cburgmer.github.io/json-path-comparison/) project.

There are also a few other features of traditional JSON Path that the specification has explicitly elected _not_ to support, such as container expressions (e.g. `$[(@.length-1)]`).  This library will strive to prioritize the specification over the comparison consensus where any conflict exists.

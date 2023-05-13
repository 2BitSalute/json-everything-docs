---
layout: "page"
title: "LiteralRule Class"
bookmark: "LiteralRule"
permalink: "/api/JsonLogic/:title/"
order: "9.10.20"
---
**Namespace:** Json.Logic.Rules

**Inheritance:**
`LiteralRule`
 🡒 
`Rule`
 🡒 
`object`

Provides a stand-in "rule" for literal values.

## Remarks

This is not exactly part of the specification, but it helps things in this library.

## Methods

### Apply(JsonNode data, JsonNode contextData)

Applies the rule to the input data.

#### Declaration

```c#
public override JsonNode Apply(JsonNode data, JsonNode contextData)
```
| Parameter | Type | Description |
|---|---|---|
| data | JsonNode | The input data. |
| contextData | JsonNode | Optional secondary data.  Used by a few operators to pass a secondary<br>    data context to inner operators. |

#### Returns

The result of the rule.


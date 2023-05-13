---
layout: "page"
title: "AdditionalPropertiesAttribute Class"
bookmark: "AdditionalPropertiesAttribute"
permalink: "/api/JsonSchema.Net.Generation/:title/"
order: "9.05.01"
---
**Namespace:** Json.Schema.Generation

**Inheritance:**
`AdditionalPropertiesAttribute`
 🡒 
`Attribute`
 🡒 
`object`

**Implemented interfaces:**

- IAttributeHandler

Applies an `additionalProperties` keyword.

## Properties

| Name | Type | Summary |
|---|---|---|
| **BoolValue** | bool? | If the attribute value represents a boolean schema, gets the boolean value. |
| **TypeValue** | Type | If the attribute value represents a type schema, gets the type. |
| **TypeId** | object |  |
## Constructors

### AdditionalPropertiesAttribute(bool boolSchema)

Creates a new **Json.Schema.Generation.AdditionalPropertiesAttribute** instance.

#### Declaration

```c#
public AdditionalPropertiesAttribute(bool boolSchema)
```
| Parameter | Type | Description |
|---|---|---|
| boolSchema | bool | A boolean schema. |

### AdditionalPropertiesAttribute(Type typeSchema)

Creates a new **Json.Schema.Generation.AdditionalPropertiesAttribute** instance.

#### Declaration

```c#
public AdditionalPropertiesAttribute(Type typeSchema)
```
| Parameter | Type | Description |
|---|---|---|
| typeSchema | Type | A type to generate the a schema for the keyword. |


---
layout: "page"
title: "PropertyDependencyJsonConverter Class"
bookmark: "PropertyDependencyJsonConverter"
permalink: "/api/JsonSchema.Net/:title/"
order: "10.01.134"
---
**Namespace:** Json.Schema

**Inheritance:**
`PropertyDependencyJsonConverter`
 🡒 
`WeaklyTypedJsonConverter<PropertyDependency>`
 🡒 
`JsonConverter<PropertyDependency>`
 🡒 
`JsonConverter`
 🡒 
`object`

**Implemented interfaces:**

- IWeaklyTypedJsonConverter

JSON converter for **Json.Schema.PropertyDependency**.

## Properties

| Name | Type | Summary |
|---|---|---|
| **HandleNull** | bool |  |
| **Type** | Type |  |

## Methods

### Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)

Reads and converts the JSON to type **Json.Schema.PropertyDependency**.

#### Declaration

```c#
public override PropertyDependency Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
```

| Parameter | Type | Description |
|---|---|---|
| reader | ref Utf8JsonReader | The reader. |
| typeToConvert | Type | The type to convert. |
| options | JsonSerializerOptions | An object that specifies serialization options to use. |


#### Returns

The converted value.

### Write(Utf8JsonWriter writer, PropertyDependency value, JsonSerializerOptions options)

Writes a specified value as JSON.

#### Declaration

```c#
public override void Write(Utf8JsonWriter writer, PropertyDependency value, JsonSerializerOptions options)
```

| Parameter | Type | Description |
|---|---|---|
| writer | Utf8JsonWriter | The writer to write to. |
| value | PropertyDependency | The value to convert to JSON. |
| options | JsonSerializerOptions | An object that specifies serialization options to use. |



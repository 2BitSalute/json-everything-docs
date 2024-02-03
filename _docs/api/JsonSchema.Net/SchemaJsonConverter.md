---
layout: "page"
title: "SchemaJsonConverter Class"
bookmark: "SchemaJsonConverter"
permalink: "/api/JsonSchema.Net/:title/"
order: "10.01.151"
---
**Namespace:** Json.Schema

**Inheritance:**
`SchemaJsonConverter`
 🡒 
`WeaklyTypedJsonConverter<JsonSchema>`
 🡒 
`JsonConverter<JsonSchema>`
 🡒 
`JsonConverter`
 🡒 
`object`

**Implemented interfaces:**

- IWeaklyTypedJsonConverter

JSON converter for **Json.Schema.JsonSchema**.

## Properties

| Name | Type | Summary |
|---|---|---|
| **HandleNull** | bool |  |
| **Type** | Type |  |

## Methods

### Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)

Reads and converts the JSON to type **Json.Schema.JsonSchema**.

#### Declaration

```c#
public override JsonSchema Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
```

| Parameter | Type | Description |
|---|---|---|
| reader | ref Utf8JsonReader | The reader. |
| typeToConvert | Type | The type to convert. |
| options | JsonSerializerOptions | An object that specifies serialization options to use. |


#### Returns

The converted value.

### Write(Utf8JsonWriter writer, JsonSchema value, JsonSerializerOptions options)

Writes a specified value as JSON.

#### Declaration

```c#
public override void Write(Utf8JsonWriter writer, JsonSchema value, JsonSerializerOptions options)
```

| Parameter | Type | Description |
|---|---|---|
| writer | Utf8JsonWriter | The writer to write to. |
| value | JsonSchema | The value to convert to JSON. |
| options | JsonSerializerOptions | An object that specifies serialization options to use. |



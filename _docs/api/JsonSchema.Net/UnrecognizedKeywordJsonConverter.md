---
layout: "page"
title: "UnrecognizedKeywordJsonConverter Class"
bookmark: "UnrecognizedKeywordJsonConverter"
permalink: "/api/JsonSchema.Net/:title/"
order: "10.01.176"
---
**Namespace:** Json.Schema

**Inheritance:**
`UnrecognizedKeywordJsonConverter`
 🡒 
`WeaklyTypedJsonConverter<UnrecognizedKeyword>`
 🡒 
`JsonConverter<UnrecognizedKeyword>`
 🡒 
`JsonConverter`
 🡒 
`object`

**Implemented interfaces:**

- IWeaklyTypedJsonConverter

JSON converter for **Json.Schema.UnrecognizedKeyword**.

## Properties

| Name | Type | Summary |
|---|---|---|
| **HandleNull** | bool |  |
| **Type** | Type |  |

## Methods

### Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)

Reads and converts the JSON to type **Json.Schema.UnrecognizedKeyword**.

#### Declaration

```c#
public override UnrecognizedKeyword Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
```

| Parameter | Type | Description |
|---|---|---|
| reader | ref Utf8JsonReader | The reader. |
| typeToConvert | Type | The type to convert. |
| options | JsonSerializerOptions | An object that specifies serialization options to use. |


#### Returns

The converted value.

### Write(Utf8JsonWriter writer, UnrecognizedKeyword value, JsonSerializerOptions options)

Writes a specified value as JSON.

#### Declaration

```c#
public override void Write(Utf8JsonWriter writer, UnrecognizedKeyword value, JsonSerializerOptions options)
```

| Parameter | Type | Description |
|---|---|---|
| writer | Utf8JsonWriter | The writer to write to. |
| value | UnrecognizedKeyword | The value to convert to JSON. |
| options | JsonSerializerOptions | An object that specifies serialization options to use. |



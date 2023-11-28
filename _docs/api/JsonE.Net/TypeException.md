---
layout: "page"
title: "TypeException Class"
bookmark: "TypeException"
permalink: "/api/JsonE.Net/:title/"
order: "10.12.010"
---
**Namespace:** Json.JsonE

**Inheritance:**
`TypeException`
 🡒 
`JsonEException`
 🡒 
`Exception`
 🡒 
`object`

**Implemented interfaces:**

- ISerializable

Thrown from **Json.JsonE.JsonE.Evaluate(System.Text.Json.Nodes.JsonNode,System.Text.Json.Nodes.JsonNode)** when ???.

## Properties

| Name | Type | Summary |
|---|---|---|
| **Data** | IDictionary |  |
| **HelpLink** | string |  |
| **HResult** | int |  |
| **InnerException** | Exception |  |
| **Message** | string |  |
| **Source** | string |  |
| **StackTrace** | string |  |
| **TargetSite** | MethodBase |  |

## Constructors

### TypeException(string message)

Creates a new instance of **Json.JsonE.TemplateException**.

#### Declaration

```c#
public TypeException(string message)
```

| Parameter | Type | Description |
|---|---|---|
| message | string | The error message. |



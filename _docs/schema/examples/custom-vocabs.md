---
layout: page
title: Extending JSON Schema Validation With Your Own Vocabularies
bookmark: Custom Vocabs
permalink: /schema/examples/:title/
icon: fas fa-tag
order: "1.4.4"
---
These examples will show how to extend JSON Schema validation by creating a new keyword and incorporating it into a new vocabulary.

> These examples are actually defined in one of the library's unit tests.
{: .prompt-info }

For a more detailed explanation about the concepts behind vocabularies, please see the Vocabularies page.

## Defining a Keyword {#example-schema-vocabs-keyword}

We want to define a new `maxDate` keyword that allows a schema to enforce a maximum date value to appear in an instance property.  We'll start with the keyword.

```c#
// The SchemaKeyword attribute is how the deserializer knows to use this
// class for the "maxDate" keyword.
[SchemaKeyword(Name)]
// Naturally, we want to be able to deserialize it.
[JsonConverter(typeof(MaxDateJsonConverter))]
// We need to declare which vocabulary this keyword belongs to.
[Vocabulary("http://mydates.com/vocabulary")]
// Specify which versions the keyword is compatible with.
[SchemaSpecVersion(SpecVersion.Draft201909 | SpecVersion.Draft202012)]
class MaxDateKeyword : IJsonSchemaKeyword, IEquatable<MaxDateKeyword>
{
    // Define the keyword in one place.
    internal const string Name = "maxDate";

    // Define whatever data the keyword needs.
    public DateTime Date { get; }

    public MaxDateKeyword(DateTime date)
    {
        Date = date;
    }

    // Implements IJsonSchemaKeyword
    public void Evaluate(EvaluationContext context)
    {
        var dateString = context.LocalInstance!.GetValue<string>();
        var date = DateTime.Parse(dateString);

        if (date > Date)
            context.LocalResult.Fail(Name, "[[provided:O]] must be on or before [[value:O]]",
                ("provided", date),
                ("value", Date));
    }

    // Equality stuff
    public bool Equals(MaxDateKeyword other)
    {
        if (ReferenceEquals(null, other)) return false;
        if (ReferenceEquals(this, other)) return true;
        return Date.Equals(other.Date);
    }

    public override bool Equals(object obj)
    {
        return Equals(obj as MaxDateKeyword);
    }

    public override int GetHashCode()
    {
        return Date.GetHashCode();
    }
}
```

We need to define that serializer, too.

```c#
class MaxDateJsonConverter : JsonConverter<MaxDateKeyword>
{
    public override MaxDateKeyword Read(ref Utf8JsonReader reader,
                                        Type typeToConvert,
                                        JsonSerializerOptions options)
    {
        // Check to see if it's a string first.
        if (reader.TokenType != JsonTokenType.String)
            throw new JsonException("Expected string");

        var dateString = reader.GetString();
        // If the parse fails, then it's not in the right format,
        // and we should throw an exception anyway.
        var date = DateTime.Parse(dateString, CultureInfo.InvariantCulture, DateTimeStyles.AssumeUniversal);

        return new MaxDateKeyword(date);
    }

    public override void Write(Utf8JsonWriter writer,
                               MaxDateKeyword value,
                               JsonSerializerOptions options)
    {
        writer.WritePropertyName(MaxDateKeyword.Name);
        writer.WriteStringValue(value.Date.ToString("yyyy'-'MM'-'dd'T'HH':'mm':'ssK"));
    }
}
```

## Registering the Keyword {#example-schema-vocabs-register-keyword}

Now that we have the keyword, we need to tell the system about it.

```c#
SchemaKeywordRegistry.Register<MaxDateKeyword>();
```

> If you're building a dynamic system where you don't always want the keyword supported, it can be removed using the `SchemaKeywordRegistry.Unregister<T>()` static method.
{: .prompt-info }

That's technically all you need to do to support a custom keyword.  However, going forward for JSON Schema, custom keywords should be defined in a custom vocabulary.

## Defining a Vocabulary {#example-schema-vocabs-definition}

Vocabularies are used within JSON Schema to ensure that the validator you're using supports your new keyword.  Because we have already created the keyword and registered it, we know it is supported.

However, we might not be implementing _our_ vocabulary.  This keyword is likely from a third party who has written a schema that declares a vocabulary that defines `maxDate`, and we're trying to support _that_.

In accordance with the specification, JsonSchema<nsp>.Net will refuse to process any schema whose meta-schema declares a vocabulary it doesn't know about.  Because of this, it won't process the third-party schema unless we define the vocabulary on our end.

```c#
static class MyCustomVocabularies
{
    // Define the vocabulary and list the keyword types it defines.
    public static readonly Vocabulary DatesVocabulary =
        new Vocabulary("http://mydates.com/vocabulary", typeof(MaxDateKeyword));

    // Although not required a vocabulary may also define a meta-schema.
    // It's a good idea to implement that as well.
    public static readonly JsonSchema DatesMetaSchema =
        new JsonSchemaBuilder()
            .Id("http://mydates.com/schema")
            .Schema(MetaSchemas.Draft202012Id)
            .Vocabulary(
                (Vocabularies.Core202012Id, true),
                ("http://mydates.com/vocabulary", true)
            )
            .Properties(
                (MaxDateKeyword.Name, new JsonSchemaBuilder()
                    .Type(SchemaValueType.String)
                    .Format(Formats.DateTime)
                )
            );
}
```

Then they need to be registered.  This is done on the schema validation options.

```c#
options.SchemaRegistry.Register(new Uri("http://mydates.com/schema"), DatesMetaSchema);
options.VocabularyRegistry.Register(DatesVocabulary);
```

And that's it.  The vocabulary and keyword are ready for use.

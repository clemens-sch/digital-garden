#Definitions 

[Main-Source](https://deep-thought.norwin.at/tech-kb/web-development/Message-Formats#:~:text=mess)

---
## # Why?

In order to being able to **exchange information**, the information needs to be encoded into something that can be sent from A to B.

We need to use message formats whenever we need to pass a barrier where A and B cannot exchange information directly on a binary level. This might be different platforms, different processes or machines that interact witch each other over network.

serialization - binary format
deserialization - read a message in object representation

![MessageExchange.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/web-development/assets/MessageExchange.excalidraw.svg)

---
## # Why? (2)

Another big advantage of message formats is, that they allow us to easily store information by creating a file and saving the encoded information into that file.

This is mainly used to persist information, such as configuration values or data, that can be used by programs at runtime.

---
## # Message format kinds

We differentiate between two main types of message formats:

- **Text Formats**
    - Using character encodings, such as UTF-8, ASCII, …
    - Text can be read and interpreted by humans
    - Information can be easily modified, using any text editor
- **Binary Formats**
    - Information is encoded as a sequence of binary data
    - Not standardized. There are different ways of encoding information as binary data.
    - Not human readable
    - Software is needed in order to read, interpret and modify the message.

---
## # Popular formats

- JSON
    - **J**ava**S**cript **O**bject **N**otation
- XML
    - E**x**tensible **M**arkup **L**anguage
- YAML
    - **Y**et **A**nother **M**arkup **L**anguage
- Protobuf
    - **Proto**col **Buf**fer

---
## # JSON

JSON was specified in the early 2000s. It is e **text format** that uses Unicode to encode characters. It was derived from JavaScript, but is an **open standard** format, that is **language-independent**.

---
## # JSON Format

The JSON Format is specified by the [ECMA-404](https://ecma-international.org/wp-content/uploads/ECMA-404_2nd_edition_december_2017.pdf) standard. It’s a very simple and easy to understand format. I recommend having a look at the specification.

---
## # JSON value

A value can be one of the following:

- **object**
- **array**
- **number**
- **string**
- _true_
- _false_
- _null_

---
## # JSON string

A **string** is a sequence of characters wrapped by quotation marks.

Contrary to JavaScript, single quotes `'` are not valid quotation marks to identify strings.

---
## # JSON number

A number is a sequence of decimal digits. It may have a preceding minus sign. It may have a fractional part prefixed by a decimal point. It may have an exponent prefixed by `e` or `E`.

Numeric values that cannot be represented as sequences of digits (such as Infinity and NaN) are not permitted.

---
## # JSON objects

Objects are defined by using braces `{}`. Inbetween the braces we specify key-value pairs in the form of **string** : **value**

```json
{
	"key": "value"
}
```

---
## # JSON array

An array structure is a pair of square brackets surrounding zero or more **values**.

```json
[12, "banana"]
```

---
## # JSON in .NET

Up until recently the most commonly used package for dealing with JSON values in .NET was [Newtonsoft Json.NET](https://www.newtonsoft.com/json). This package can be included via NuGet.
- not needed nowadays.

Since .NET Core 3.1 **System.Text.Json** is included in the runtime. It focuses primarily on performance, security and standards compliance. Unless you have a niche need (e.g. dealing with malformed JSON) System.Text.Json is the recommended way.

---
## # System.Text.Json - Serialization

```csharp
using System.Text.Json;

var weatherForecast = new WeatherForecast
{
	Date = DateTime.Parse("2019-08-01"),
	TemperatureCelsius = 25,
	Summary = "Hot"
};

string jsonString = JsonSerializer
	.Serialize(weatherForecast);
Console.WriteLine(jsonString);

public class WeatherForecast
{
	public DateTimeOffset Date { get; set; }
	public int TemperatureCelsius { get; set; }
	public string? Summary { get; set; }
}
```

The output of the serialized JSON will look as the following:

```json
{ 
	"Date": "2019-08-01T00:00:00+00:00", 
	"TemperatureCelsius": 25, 
	"Summary": "Hot" 
}
```

---
## # System.Text.Json - Serialization Behavior

- By default, all public properties are serialized. You can  [specify properties to ignore](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/ignore-properties). You can also include  [private members](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/immutability#non-public-members-and-property-accessors).
	- if not: set `[JsonIgnore]` before the property
- The  [default encoder](https://learn.microsoft.com/en-us/dotnet/api/system.text.encodings.web.javascriptencoder.default#system-text-encodings-web-javascriptencoder-default) escapes non-ASCII characters, HTML-sensitive characters within the ASCII-range, and characters that must be escaped according to  [the RFC 8259 JSON spec](https://tools.ietf.org/html/rfc8259#section-7).
- By default, JSON is minified. You can  [pretty-print the JSON](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/how-to#serialize-to-formatted-json).
	- all in one line when serialized.
- By default, casing of JSON names matches the .NET names. You can  [customize JSON name casing](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/customize-properties).
- By default, circular references are detected and exceptions thrown. You can  [preserve references and handle circular references](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/preserve-references).
- By default,  [fields](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/fields) are ignored. You can  [include fields](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/fields).

---
## # System.Text.Json - Deserialization

```csharp
using System.Text.Json;

string jsonString = 
@"{
  ""Date"": ""2019-08-01T00:00:00-07:00"",
  ""TemperatureCelsius"": 25,
  ""Summary"": ""Hot""
}";

WeatherForecast? weatherForecast = JsonSerializer
	.Deserialize<WeatherForecast>(jsonString);

public class WeatherForecast
{
	public DateTimeOffset Date { get; set; }
	public int TemperatureCelsius { get; set; }
	public string? Summary { get; set; }
}
```

Nun hätte man auf die deserialisierten Daten zugriff:

```csharp
var? weatherForecast = JsonSerializer.Deserialize<WeatherForecast>(jsonString); 

if (weatherForecast != null) 
{ 
	Console.WriteLine($"Date: {weatherForecast.Date}");
	Console.WriteLine($"Temperature Celsius: {weatherForecast.TemperatureCelsius}"); 
	Console.WriteLine($"Summary: {weatherForecast.Summary}"); 
}

// Output
Date: 8/1/2019 7:00:00 AM -07:00
Temperature Celsius: 25
Summary: Hot
```

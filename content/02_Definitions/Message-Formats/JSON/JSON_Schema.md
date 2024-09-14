#Definitions 

[[Message-Formats]]

[JSON-Validator](https://www.jsonschemavalidator.net/)

---
## # What is a Schema ?

We specify/write a schema to annotate and validate JSON documents.
In case, we only want to accept specific data.
If the data input would be nonsense, we will throw an error.

A schema is the document, that contains the description of the data.

---
## # Overview

In this example we use a product catalog, that stores its data using JSON objects, like the following:

```json
{
	"productId": 1,
	"productName": "Green door",
	"price": 12.99,
	"tags": [ "home", "door", "green" ]
}
```

In this catalog, each product has its own identifier.
This object is readable for humans. But we don't specifically know the structure, constraints, data types of for this set of JSON data.
- So we gonna need a JSON schema.

---
## # Basic Schema Keywords

```json
{
  // draft of JSON schema standard
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  // sets URI to schema
  "$id": "https://example.com/product.schema.json",
  // additional information for schema - no constraints to validated data
  "title": "Product",
  "description": "A product in the catalog",
  // data type for JSON schema
  "type": "object"
}
```

---
### # Properties Keyword

`properties` Is a validation keyword. 
When you define properties, you create an object where each property represents a key in the JSON data that's beeing validated.

```json
{
	"$schema": "https://json-schema.org/draft/2020-12/schema",
	"title": "Product",
	"description": "A product from my catalog",
	"type": "object",
	"properties": {
		"productId": {
			"description": "The Id for a product",
			"type": "integer"
		},
		"productName": {
			"description": "Name of the product",
			"type": "string"
		},
		"price": {
			"description": "Price of product",
			"type": "number",
			// only values > 0 are considered as valid
			"exclusiveMinimum": 0
		}
	}
}
```

---
### # Required Properties

How to specify, that certain properties are required.
Add .>required keyword.

```json
{
	"$schema": "https://json-schema.org/draft/2020-12/schema",
	"title": "Product",
	"description": "A product from my catalog",
	"type": "object",
	"required": [ "productId", "productName" ],
	"properties": {
		...
	}
}
```

---
### # Optional Properties

How to specify, that certain properties are optional.
Add properties>... keyword - do not set it as required.

```json
{
	...
	"required": [ "productId", "productName", "price" ],
	"properties": {
		"productId": {
			"description": "The Id for a product",
			"type": "integer"
		},
		"productName": {
			"description": "Name of the product",
			"type": "string"
		},
		"price": {
			"description": "Price of product",
			"type": "number",
			"exclusiveMinimum": 0
		},
		"tags": {
			"description": "Tags for the product",
			"type": "array",
			"items": {
				"type": "string"
			},
			// making sure, that there is at least one item in arr
			"minItems": 1,
			// making sure, that every item is unique
			"uniqueItems": true
		}
	}
}

```




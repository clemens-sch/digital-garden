#Definitions 

----
## # What is JSON ?

`JavaScript Object Notation`

...

---
### # JSON Schema

Example for a json-schema (according to drawing):

![[WhatsApp Bild 2024-03-14 um 08.50.11_875d3021.jpg]]

```json
{
   "$schema":"https://json-schema.org/draft/2019-09/schema",
   "type":"object",
   "title":"school",
   "required":[
      "name",
      "classes",
      "teachers"
   ],
   "properties":{
      "name":{
         "type":"string",
         "const":"HTL Krems"
      },
      "classes":{
         "type":"array",
         "minItems":5,
         "items":{
            "type":"string"
         }
      },
      "teachers":{
         "type":"array",
         "minItems":1,
         "items":{
            "type":"object",
            "required":[
               "name",
               "subjects"
            ],
            "properties":{
               "name":{
                  "type":"string"
               },
               "subjects":{
                  "type":"array",
                  "minItems":1,
                  "items":{
                     "type":"string"
                  }
               }
            }
         }
      }
   }
}
```


With Examples:

```json
{
  "name": "HTL Krems",
  "classes": ["1CHIT", "2CHIT", "3CHIT", "4CHIT", "5CHIT"],
  "teachers": [
    {
      "name": "Herwig Macho",
      "subjects": ["INSY", "SEW"]
    },
    {
      "name": "Frau Perr",
      "subjects": ["Deutsch"]
    }
  ]
}
```

---
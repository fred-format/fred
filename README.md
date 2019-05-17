# Flexible REpresentation of Data (Fred)

Specification of the Fred (Flexible REpresentation of Data) format

## What is Fred?

FRED (Flexible REpresentation of Data) is a data-interchange format. It was created with the goal to be easy for humans to read and write but also easy to create parsers.

It has more data types than JSON and some features like support for metadata and tags.

## How does it look like?

### String

The only way to represent a string is with quotation marks. 

```fred
"String that is valid. \"quote\" are allowed. Unicode \uOOE9 and other escape\n\t"
```

It accepts any unicode character except those that need to be escaped.

```
\b         
\t        
\n        
\f         
\r         
\"         
\\         
\xXX
\uXXXX     
\UXXXXXXXX
```

### Number

Number representation can be both Integer or Float numbers.

#### Integer


```fred
[ 
  42 
  -42 
  1_000_000 
  0xBEEF_00E9 
  0o7823 
  0b1010 
]
```

#### Float

```fred
[ 
  42.12 
  -42.12 
  4.32e-10 
  -2E-2 
]
```

### Bool

Booleans are always lowercase.

```fred
true
false
```

### DateTime

Date can be representended with only the date portion of an RFC 3339 formatted date-time.
This has no timezone or offset associated.
```fred
1989-10-14
```

Time can be representended with only the time portion of an RFC 3339 formatted date-time without timezone.
This has no timezone or offset associated.
```fred
14:35:54.83
```

DateTime can be represented without timezone
```fred
1989-10-14T14:35:54.83
```

DateTime can have the '_' as the separator to improve readability.
```fred
1989-10-14_14:35:54.83
```


DateTime can be represented with timeoffset or 'Z' as UTC
```fred
1989-10-14T14:35:54.83Z
```

```fred
1989-10-14T14:35:54.83-03:00
```

### Blob

It can represents raw binary data. It starts with the '#' character followed by a blob string
representing the binary data.

```fred
#"dffdtr54123asda1yhn7"
```

### Symbols

Also represent symbols with the '$' character.

```fred
$var1
```

### Tags

```fred
tag [1 2 3]
```

```fred
(tag attr=1)
```
### Metadata

```fred
phone (country=55) "32131123"
```

```fred
blog (page=1) [{ title: "LOREM IPSUM" }]
```


### Commas are whitespace

```fred
my-app.users.name (attr="string" attr2=42 ) {
  foo: "bar",
  bar: "foo",
}
```

### Streaming

```fred
---
person "Jhon"
---
person "Mary"
---
```

## Fred vs. JSON



## Fred vs. XML

## Fred vs. other formats

* Edn https://github.com/edn-format/edn
* TJSON
* JSON Schema
* Toml
* YAML
* Ion https://amzn.github.io/ion-docs/
* ...?

## Fred encodings

### HTML

```fred
div [
    h1 "Hello world!"
    p "hi there"
]
```

## Grammar

```
document : stream
         | value

stream : "---" (value  "---")*

value : tagged
      | atom

tagged : name [attrs] atom
       | "(" name attr* ")" 

attrs : "(" meta_item* ")"

attr : name "=" atom

atom : object
     | array
     | dateTime
     | symbol
     | number
     | string
     | bool
     | "null"

object : "{" pair* "}"

pair : name ":" value

array : "[" atom* "]"

bool : "true" | "false"

symbol : "$" name

number : NUMBER_LITERAL 
       | HEX_LITERAL
       | OCT_LITERAL
       | BIN_LITERAL 

dateTime : date
       | TIME_FORMAT

date : DATE_FORMAT [ ("_" | "T") TIME_FORMAT TIME_OFFSET]

string : STRING_LITERAL
       | blob

blob : "#" BLOB_LITERAL

name : VARIABLE
     | QUOTED_VARIABLE

VARIABLE : /[^#\"`$:;{}\[\]=\(\)\t\r\n ,0-9]{1}[^#\"`$:;{}\[\]=\(\)\t\r\n ,]*/

QUOTED_VARIABLE : /`(?:[^\\`]|\\(?:[bfnrtv`\\/]|x[0-9a-fA-F]|u[0-9a-fA-F]{4}|U[0-9a-fA-F]))*`/

STRING_LITERAL : /"(?:[^\\"]|\\(?:[bfnrtv"\\/]|x[0-9a-fA-F]|u[0-9a-fA-F]{4}|U[0-9a-fA-F]))*"/

BLOB_LITERAL : /"(?:[^\\"\u\U]|\\(?:[bfnrtv"\\/]|x[0-9a-fA-F]))*"/

NUMBER_LITERAL : /-?(0|[1-9]\d*)(\.\d+)?([eE][+-]?\d+)?/

HEX_LITERAL : /0x[0-9a-fA-F]{1}([0-9a-fA-F]{1}|_[0-9a-fA-F]{1})*/

OCT_LITERAL : /0o[0-8]{1}([0-8]{1}|_[0-8]{1})*/

BIN_LITERAL : /0b[01]{1}([01]{1}|_[01]{1})*/

DATE_FORMAT : /\d{4}-\d{2}-\d{2}/

TIME_FORMAT : /\d{2}:\d{2}:\d{2}(\.\d+)?/

TIME_OFFSET : /Z|[+-]\d{2}:\d{2}/

/* SKIPPED */

WHITESPACE : /[ \t\n\r,]+/

COMMENT : /;.*/

```

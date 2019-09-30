# Flexible REpresentation of Data (FRED)

Specification of the FRED (Flexible REpresentation of Data) format

## What is FRED?

FRED (Flexible REpresentation of Data) is a data-interchange format. It was created with the goal to be easy for humans to read and write but also easy to create parsers.

It has more data types than JSON and some features like support for metadata and tags.

## How does it look like?

### null value

null values are represented as expected.

```fred
null
```

### Boolean

Booleans are always lowercase.

```fred
true
false
```

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

It can represents raw binary date with backticks

```fred
`dffdtr54123asda1yhn7`
```

### Symbols

Also represent symbols with the '$' character.

```fred
$var1
```

### Array

Array expect at least one whitespace to separate the values. Commas are also whitespace.

```fred
[1 2 3]
```

```fred
[1,2,3]
```

### Object or Dictionary

The key can be represented without quotation marks, but also as a literal string with double quotes.

The separator between values is  at least one whitespace, Commas are also whitespace.

```fred
{
  foo : "bar"
  "test foo" : "bar"
}
```

### Tags

A tag in FRED indicates some semantic meaning to the following element.

```fred
person {
  name: "eric"
  age: 25
}
```

```fred
fibonacci [0 1 1 2 3 5 8 13]
```

### Metadata

With tagged elements it is also possible to add metadata abot the following element.

```fred
phone (country="Brazil") "32131123"
```

```fred
blog (page=1) [{ title: "LOREM IPSUM" }]
```

It is also possible to represent a tag and meta data without a following element with the notation

```fred
(tag attr=1)
```

### Commas are whitespace

In FRED commas are treated as whitespace.

```fred
my-app.users.name (attr="string" attr2=42 ) {
  foo: "bar",
  bar: "foo",
}
```

### Streaming

FRED has support for streaming.

```fred
#. person "Jhon"
#. person "Mary"
#. person "James"
```

## FRED vs. JSON

FRED extends the JSON data model allowing more complex types. It also has an explicit mechanism for
extension of the data model (tags).

## FRED vs. XML

FRED has a mechanism of extension inspired by XML. But differently from XML it 
has the goal of being a data-exchange format not a markup language. 
Also, it will have a schema format.

## Grammar

The grammar for FRED is defined in a [grammar file](https://github.com/fred-format/fred/blob/master/grammar.lark) which is a lark file and follows the
notation specified [here](https://lark-parser.readthedocs.io/en/latest/grammar/).
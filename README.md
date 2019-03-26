# Flexible REpresentation of Data (Fred)

Specification of the Fred (Flexible REpresentation of Data) format

## What is Fred?

## How does it look like?

* Commas are whitespace

```fred
my-app.users.name (attr="string" attr2=42 ) {
  foo: "dfofpodk"
  "dijfiodjfo"
  "idjfigdfg" 
}
```

## Fred vs. Json

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

```
div [
    h1 "Hello world!"
    p "hi there"
]
(br class="fdofod")
```


## Schema


## Grammar

```
document : "Stream" value*
         | value

value : tagged
      | atom

atom : object
     | array
     | DATE? Date "50-12-32"
     | URI?  uri "http://github.com"
     | Symbols? `foo `bar? Symb "dfjdofgjfd"
     | Hash? #md5 `dfdiogjdofig`
     | BINARY? "" '' ``
     | STRING
     | NUMBER int/float
     | 0xFF, 0o123, 0b1010101 ??
     | "true"
     | "false"
     | "null"

tagged : tag [meta] atom
       | "(" tag meta_item* ")" 

meta : "(" meta_item* ")"

meta_item : name "=" atom -> pair
          | name          -> boolean_value

array : "[" (atom [","])* "]"

object : "{" (pair [","])* "}"

pair : name ":" value

name : VARIABLE
     | QUOTED_VARIABLE
WHITESPACE : /[\s,]/

```

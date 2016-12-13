---
title: JSON Table Schema
layout: spec
listed: true
version: 1.0-pre15
updated: 17 November 2016
created: 12 November 2012
mediatype: application/vnd.tableschema+json
summary: This RFC defines a simple schema for tabular data. The schema is
  designed to be expressible in JSON.
ietf-keywords: true
---


### Changelog

- 1.0.0-pre15: add calendar units `gyear` and `gyearmonth` ([#105](https://github.com/dataprotocols/dataprotocols/issues/105)), tweak pattern support for date/time types [#260](https://github.com/frictionlessdata/specs/issues/260)
- 1.0.0-pre14: add support for `missingValue` ([#97](https://github.com/dataprotocols/dataprotocols/issues/97))
- 1.0.0-pre13: remove `null` datatype ([#262](https://github.com/dataprotocols/dataprotocols/issues/262))
- 1.0.0-pre12: add support for new number properties such as `decimalChar`([#246](https://github.com/dataprotocols/dataprotocols/issues/246))
- 1.0.0-pre11: add new field property: rdfType ([#217](https://github.com/dataprotocols/dataprotocols/issues/217))
- 1.0.0-pre10: add new field types: duration ([#210](https://github.com/dataprotocols/dataprotocols/issues/210))
- 1.0.0-pre9: make date formats stricter for default
  [issue](https://github.com/dataprotocols/dataprotocols/issues/104). Define
  value of fmt:PATTERN for dates
  [issue](https://github.com/dataprotocols/dataprotocols/issues/200)
- 1.0-pre8: Rename contraints.oneOf to contraints.enum [issue](https://github.com/dataprotocols/dataprotocols/issues/191)
- 1.0-pre7: Add contraints.oneOf [issue](https://github.com/dataprotocols/dataprotocols/issues/175)
- 1.0-pre6: clarify types and formats
  [issue](https://github.com/dataprotocols/dataprotocols/issues/159)
- 1.0-pre5: add type validation
  [issue](https://github.com/dataprotocols/dataprotocols/issues/95)
- 1.0-pre4: add foreign key support - see this
  [issue](https://github.com/dataprotocols/dataprotocols/issues/23)
- 1.0-pre3.2: add primary key support (see this
    [issue](https://github.com/dataprotocols/dataprotocols/issues/21))
- 1.0-pre3.1: breaking changes.
  - `label` (breaking) changed to `title` - see [Closer alignment with JSON
    Schema](https://github.com/dataprotocols/dataprotocols/issues/46)
  - `id` changed to `name` (with slight alteration in semantics - i.e. SHOULD
    be unique but no longer MUST be unique)

### Table of Contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Concepts

A Table consists of a set of rows. Each row has a set of fields
(columns). We usually expect that each Row has the same set of fields
and thus we can talk about *the* fields for the table as a whole.

In cases of tables in spreadsheets or CSV files we often interpret the
first row as a header row giving the names of the fields. By contrast,
in other situations, e.g. tables in SQL databases the fields (columns)
are explicitly designated.

To illustrate here's a classic spreadsheet table:

    field     field
      |         |
      |         |
      V         V

     A     |    B    |    C    |    D      <--- Row
     ------------------------------------
     valA  |   valB  |  valC   |   valD    <--- Row
     ...

In JSON a table would be:

    [
      { "A": value, "B": value, ... },
      { "A": value, "B": value, ... },
      ...
    ]

# Specification

A JSON Table Schema consists of:

* a required list of _field descriptors_
* optionally, a _primary key_ description
* optionally, a _foreign _key_ description

A schema is described using JSON. This might exist as a standalone document
or may be embedded within another JSON structure, e.g. as part of a
data package description.


A JSON Table Schema descriptor `MUST` have a media type of `{{ page.mediatype }}`.

## Schema

A schema has the following structure:

    {
      # fields is an ordered list of field descriptors
      # one for each field (column) in the table
      "fields": [
        # a field-descriptor
        {
          "name": "name of field (e.g. column name)",
          "title": "A nicer human readable label or title for the field",
          "type": "A string specifying the type",
          "format": "A string specifying a format",
          "description": "A description for the field"
          ...
        },
        ... more field descriptors
      ],
      # (optional) specification of the primary key
      "primaryKey": ...
      # (optional) specification of the foreign keys
      "foreignKeys": ...

    }

That is, a JSON Table Schema is:

-   a Hash which `MUST` contain a key `fields`
-   `fields` MUST be an array where each entry in the array is a field
    descriptor. (Structure and usage described below)
-   the Hash `MAY` contain a property `primaryKey` (structure and usage
    specified below)
-   the Hash `MAY` contain a property `foreignKeys` (structure and usage
    specified below)
-   the Hash `MAY` contain any number of other properties (not defined in this
    spec)

## Field Descriptors

A field descriptor is a simple JSON hash that describes a single field. The
descriptor provides additional human-readable documentation for a field, as
well as additional information that may be used to validate the field or create
a user interface for data entry.

At a minimum a field descriptor will contain at least a `name` key, but MAY
have additional keys as described below:

    {
      "name": "name of field (e.g. column name)",
      "title": "A nicer human readable label or title for the field",
      "type": "A string specifying the type",
      "format": "A string specifying a format",
      "missingValue": "",
      "description": "A description for the field",
      "constraints": {
          # a constraints-descriptor
      }
    }

-   a field descriptor MUST be a Hash
-   the field descriptor Hash MUST contain a `name` property. This
    property `SHOULD` correspond to the name of field/column in the
    data file (if it has a name). As such it `SHOULD` be unique (though
    it is possible, but very bad practice, for the data file to have
    multiple columns with the same name). Additionally, `name` `SHOULD`
    be considered case sensitive. In practice, case sensitivity for names
    can be limiting in certain scenarios, so consumers `MAY` choose to ignore
    case sensitivity for `name` values.
-   the field descriptor Hash MAY contain any number of other properties
-   specific properties that MAY be included in the Hash and whose
    meaning is defined in this spec are:

    -   `title`: A nicer human readable label or title for the field
    -   `description`: A description for this field e.g. "The recipient of
        the funds"
    -   `type`: The type of the field (string, number etc) - see below for
        more detail. If type is not provided a consumer should assume a type of
        "string".
    -   `format`: A description of the format e.g. "DD.MM.YYYY" for a
        date. See below for more detail.
    -   `missingValue`: a field value which should be interpreted as missing data (`null`). See below for
    -   `constraints`: A constraints descriptor that can be used by consumers
        to validate field values

### Field Constraints

A set of constraints can be associated with a field. These constraints can be used
to validate data against a JSON Table Schema. The constraints might be used by consumers
to validate, for example, the contents of a data package, or as a means to validate
data being collected or updated via a data entry interface.

A constraints descriptor is a JSON hash. It `MAY` contain any of the following
keys.

- `required` -- A boolean value which indicates whether a field must have a value
  in every row of the table. An empty string is considered to be a missing value.
- `minLength` -- An integer that specifies the minimum length of a value. Supported field types are sequences, such as `string` and `array`, and collections containing items, such as `object`.
- `maxLength` -- An integer that specifies the maximum length of a value. Supported field types are sequences, such as `string` and `array`, and collections containing items, such as `object`.
- `unique` -- A boolean. If `true`, then all values for that field MUST be unique within the
  data file in which it is found. This defines a unique key for a row although a row could
  potentially have several such keys.
- `pattern` -- A regular expression that can be used to test field values. If the regular
  expression matches then the value is valid. Values will be treated as a string of characters.
  It is recommended that values of this field conform to the standard
  [XML Schema regular expression syntax](http://www.w3.org/TR/xmlschema-2/#regexs). See also
  [this reference](http://www.regular-expressions.info/xml.html).
- `minimum` -- specifies a minimum value for a field. This is different to `minLength` which
  checks the number of items in the value. A `minimum` value constraint checks whether a field value is greater than or equal to the specified value. The range checking depends on the `type` of the field. E.g. an integer field may have a minimum value of 100; a date field might have a minimum date. If a `minimum` value constraint is specified then the field descriptor `MUST` contain a `type` key. Supported field types are `integer`, `number`, `date`, `time` and `datetime`.
- `maximum` -- as above, but specifies a maximum value for a field.
- `enum` -- An array of values, where each value `MUST` comply with the type and format of the field.
The field value must exactly match a value in the `enum` array.

The constraints listed above may also define a list of supported field types. Implementations `SHOULD` report an error if an attempt is made to evaluate a value against an unsupported constraint.

A constraints descriptor may contain multiple constraints, in which case a consumer `MUST` apply
all the constraints when determining if a field value is valid.

A data file, e.g. an entry in a data package, is considered to be valid if all of its fields are valid
according to their declared `type` and `constraints`.

### Field Types and Formats

A field's `type` property is a string indicating the type of this field.

A field's `format` property is a string, being a keyword indicating a
format for the field type.

Both `type` and `format` are optional: in a field descriptor, the absence of a
`type` property indicates that the field is of the type "string", and the
absence of a `format` property indicates that the field's type `format` is
"default".

Types are based on the [type set of
json-schema](http://tools.ietf.org/html/draft-zyp-json-schema-03#section-5.1)
with some additions and minor modifications (cf other type lists include
those in [Elasticsearch
types](http://www.elasticsearch.org/guide/reference/mapping/)).

The type list with associated formats and other related properties is as
follows.

#### string

The field contains strings, that is, sequences of characters.

`format`:

* **default**: any valid string.
* **email**: A valid email address.
* **uri**: A valid URI.
* **binary**: A base64 encoded string representing binary data.
* **uuid**: A string that is a uuid.

#### number

The field contains numbers of any kind including decimals.

The lexical formatting follows that of decimal in [XMLSchema][xsd-decimal]: a
non-empty finite-length sequence of decimal digits separated by a period as a
decimal indicator. An optional leading sign is allowed. If the sign is omitted,
"+" is assumed. Leading and trailing zeroes are optional. If the fractional
part is zero, the period and following zero(es) can be omitted. For example:
'-1.23', '12678967.543233', '+100000.00', '210'. 

The following special string values are permitted (case need not be respected):

* NaN: not a number
* INF: positive infinity
* -INF: negative infinity

A number MAY also have a trailing:

* exponent: this MUST consist of an E followed by an optional + or - sign
  followed by one or more decimal digits (0-9)
* percentage: the percentage sign: "%. In conversion percentages should be
  divided by 100.

If both exponent and percentages are present the percentage MUST follow the
exponent e.g. '53E10%' (equals 5.3).

This lexical formatting may be modified using these additional properties:

* **decimalChar**: A string whose value is used to represent a decimal point
  within the number. The default value is ".".
* **groupChar**: A string whose value is used to group digits within the
  number. The default value is null. A common value is "," e.g. "100,000".
* **currency**: A number that may include additional currency symbols.

`format`: no options (other than the default).

[xsd-decimal]: https://www.w3.org/TR/xmlschema-2/#decimal

#### integer

The field contains integers - that is whole numbers.

Integer values are indicated in the standard way for any valid integer.

`format`: no options (other than the default).

#### boolean

The field contains boolean (true/false) data.

Boolean values can be indicated with the following strings (case-insensitive
so, for example, "Y" and "y" are both acceptable):

* **true**: 'yes', 'y', 'true', 't', '1'
* **false**: 'no', 'n', 'false', 'f', '0'

`format`: no options (other than the default).

#### object

The field contains data which is valid JSON.

`format`: no options (other than the default).

#### array

The field contains data that is a valid JSON format arrays.

`format`: no options (other than the default).

#### date

**datetime**; **date**; **time**

Tthe field contains temporal values such as dates, times and date-times.

`format`: (`datetime`, `date` and `time` share the these same options)

* **default**: An ISO8601 format string.
  * date: This MUST be in ISO8601 format YYYY-MM-DD
  * datetime: a date-time. This MUST be in ISO 8601 format of YYYY-MM-DDThh:mm:ssZ in UTC time
  * time: a time without a date
* **any**: Any parsable representation of the type. The implementing
  library can attempt to parse the datetime via a range of strategies.
  An example is `dateutil.parser.parse` from the `python-dateutils`
  library.
* **{PATTERN}**: date/time values in this field can be parsed according to
  `{PATTERN}`. `{PATTERN}` MUST follow the syntax of [standard Python / C
  strptime][strptime]. (That is, values in the this field should be parseable
  by Python / C standard `strptime` using `PATTERN`).  Example: `%d %b %y`
  would correspond to dates like: `30 Nov 14`

#### gyear

A calendar year as per [XMLSchema `gYear`][xsd-gyear].

Usual lexical reprentation is `YYYY`. There are no format options.

[xsd-gyear]: https://www.w3.org/TR/xmlschema-2/#gYear

#### gyearmonth

A specific month in a specific year as per [XMLSchema
`gYearMonth`][xsd-gyearmonth].

Usual lexical representation is: `YYYY-MM`. There are no format options.

[xsd-gyearmonth]: https://www.w3.org/TR/xmlschema-2/#gYearMonth

#### duration

A duration of time.

The lexical representation for duration is the [ISO 8601][iso8601-duration]
extended format PnYnMnDTnHnMnS, where nY represents the number of years, nM the
number of months, nD the number of days, 'T' is the date/time separator, nH the
number of hours, nM the number of minutes and nS the number of seconds. The
number of seconds can include decimal digits to arbitrary precision. Date and
time elements including their designator may be omitted if their value is zero,
and lower order elements may also be omitted for reduced precision. Here we
follow the definition of [XML Schema duration datatype][xsd-duration] directly
and that definition is implicitly inlined here.

`format`: no options (other than the default).

#### geopoint

The field contains data describing a geographic point.

`format`:

* **default**: A string of the pattern "lon, lat", where `lon` is the longitude
  and `lat` is the latitude.
* **array**: An array of exactly two items, where each item is either a number,
  or a string parsable as a number, and the first item is `lon` and the second
  item is `lat`.
* **object**: A JSON object with exactly two keys, `lat` and `lon`

#### geojson

The field contains a JSON object according to GeoJSON or TopoJSON spec.

`format`:

* **default**: A geojson object as per the [GeoJSON spec](http://geojson.org/).
* **topojson**: A topojson object as per the [TopoJSON spec](https://github.com/topojson/topojson-specification/blob/master/README.md)

#### any

Any `type` or `format` is accepted.

[strptime]: https://docs.python.org/2/library/datetime.html#strftime-strptime-behavior
[iso8601-duration]: https://en.wikipedia.org/wiki/ISO_8601#Durations
[xsd-duration]: http://www.w3.org/TR/xmlschema-2/#duration

### Missing Values

By "missing" we simply mean null or "not present for whatever reason". Many
datasets arrive with missing data values, either because a value was not
collected or it never existed.

Missing values may be indicated simply by the value being empty in other cases
a special value may have been used e.g. `-`, `NaN`, `0`, `-9999` etc.

The `missingValue` property provides a way to indicate that these values should
be interpreted as equivalent to null.

Strings that indicate missing (null) data values for a field `MAY` be provided
for a field using the the `missingValue` property.

If present, the `missingValue` MUST be a single string or an array of strings,
for example:

    "missingValue": ""
    "missingValue": "-"
    "missingValue": ["Nan", "-"]

**Note**: `missingValue` are strings rather than being the data type of
the particular field. This allows for comparison prior to casting and for
fields to have missing value which are not of their type, for example a
`number` field to have missing values indicated by `-`.

The default value of `missingValue` for a non-string type field is the empty
string `""`. For string type fields there is no default for `missingValue` (for
string fields the empty string `""` is a valid value and need not indicate
null).

**Processing**: if a missing value is encountered it SHOULD be converted into
the NULL or equivalent value.


### Rich Field Types

A richer, "semantic", description of the "type" of data in a given column MAY
be provided using a `rdfType` property on a field descriptor.

The value of of the `rdfType` property MUST be the URI of a RDF Class, that is an instance or subclass of [RDF Schema Class object][rdfs-class]

Here is an example using the Schema.org RDF Class `http://schema.org/Country`:

| Country | Year Date | Value |
| ------- | --------- | ----- |
| US      | 2010      | ...   |


    # JSON Table Schema
    {
      fields: [
        {
          "name": "Country",
          "type": "string",
          "rdfType": "http://schema.org/Country"
        }
        ... 
      }
    }

[rdfs-class]: https://www.w3.org/TR/rdf-schema/#ch_class


## Primary Key

A primary key is a field or set of fields that uniquely identifies each row in
the table.

The `primaryKey` entry in the schema Hash is optional. If present it specifies
the primary key for this table.

The `primaryKey`, if present, MUST be:

* Either: an array of strings with each string corresponding to one of the
  field `name` values in the `fields` array (denoting that the primary key is
  made up of those fields). It is acceptable to have an array with a single
  value (indicating just one field in the primary key). Strictly, order of
  values in the array does not matter. However, it is RECOMMENDED that one
  follow the order the fields in the `fields` has as client applications may
  utitlize the order of the primary key list (e.g. in concatenating values
  together).
* Or: a single string corresponding to one of the field `name` values in
  the `fields` array (indicating that this field is the primary key). Note that
  this version corresponds to the array form with a single value (and can be
  seen as simply a more convenient way of specifying a single field primary
  key).

Here's an example:

      "fields": [
        {
          "name": "a"
        },
        ...
      ],
      "primaryKey": "a"

Here's an example with an array primary key:

    "schema": {
      "fields": [
        {
          "name": "a"
        },
        {
          "name": "b"
        },
        {
          "name": "c"
        },
        ...
      ],
      "primaryKey": ["a", "c"]
     }

## Foreign Keys

<div class="alert alert-success">
Foreign Keys by necessity must be able to reference other data objects. These data objects require a specific structure for the spec to work. Therefore, this spec makes one of two assumptions:
<ul>
<li>Your Foreign Key points to another field/fields within the current JSON
Table Schema. In this case you use a special <code>self</code> keyword is
employed.</li>
<li>Your Foreign Key points to a field/fields in a JSON Table Schema
"elsewhere". In this case, the JSON Table Schema must be inside of a resource
on <a href="/data-packages/">Data Package</a>. There are two situations here:
EITHER this JSON Table Schema is already situated within a (Tabular) Data
Package and the reference is to a resource within this Data Package; OR we are
pointing out to a (Tabular) Data Package stored elsewhere e.g. online.</li>
</ul>
</div>

A foreign key is a reference where entries in a given field (or fields) on this
table ('resource' in data package terminology) is a reference to an entry in a
field (or fields) on a separate resource.

The `foreignKeys` property, if present, `MUST` be an Array. Each entry in the
array must be a `foreignKey`. A `foreignKey` `MUST` be a Hash and:

* `MUST` have a property `fields`. `fields` is a string or array specifying the
  field or fields on this resource that form the source part of the foreign
  key. The structure of the string or array is as per `primaryKey` above.
* `MUST` have a property `reference` which MUST be a Hash. The Hash
  * `MAY` have a property `datapackage`. This property is a string being a url
    pointing to a Data Package or is the name of a datapackage. If absent the
    implication is that this is a reference to a resource within the current
    data package or this is self-referencing foreign key.
  * `MUST` have a property `resource` which is the name of the resource within
    the referenced data package. For self-referencing foreign keys, the value
    of `resource` `MUST` be `self`.
  * `MUST` have a property `fields` which is a string if the outer `fields` is a
    string, else an array of the same length as the outer `fields`, describing the
    field (or fields) references on the destination resource. The structure of
    the string or array is as per `primaryKey` above.

Here's an example:


      "fields": [
        {
          "name": "state"
        }
      ],
      "foreignKeys": [
        {
          "fields": "state"
          "reference": {
            "datapackage": "http://data.okfn.org/data/mydatapackage/",
            "resource": "the-resource",
            "fields": "state_id"
          }
        }
      ]

An example of a self-referencing foreign key:

      "fields": [
        {
          "name": "parent"
        },
        {
          "name": "id"
        }
      ],
      "foreignKeys": [
        {
          "fields": "parent"
          "reference": {
            "datapackage": "",
            "resource": "self",
            "fields": "id"
          }
        }
      ]

----

# Appendix: Related Work

See <a href="{{site.baseurl}}/data-formats/">Web-Oriented Data Formats</a>  for more details and
links for each format.

-   SQL
-   DSPL
-   JSON-Stat
-   [Google
    BigQuery](https://developers.google.com/bigquery/docs/import#jsonformat)
    (JSON format section)

## DSPL

See <https://developers.google.com/public-data/docs/schema/dspl18>.
Allowed values:

-   string
-   float
-   integer
-   boolean
-   date
-   concept

## Google BigQuery

Example schema:

    "schema": {
      "fields":[
         {
            "mode": "nullable",
            "name": "placeName",
            "type": "string"
         },
         {
            "mode": "nullable",
            "name": "kind",
            "type": "string"
         },  ...
       ]
     }

Types:

-   string - UTF-8 encoded string up to 64K of data (as opposed to 64K
    characters).
-   integer - IEEE 64-bit signed integers: [-263-1, 263-1]
-   float - IEEE 754-2008 formatted floating point values
-   boolean - "true" or "false", case-insensitive
-   record (JSON only) - a JSON object; also known as a nested record

## XML Schema

See <http://www.w3.org/TR/xmlschema-2/#built-in-primitive-datatypes>

* string
* boolean
* decimal
* float
* double
* duration
* dateTime
* time
* date
* gYearMonth
* gYear
* gMonthDay
* gDay
* gMonth
* hexBinary
* base64Binary
* anyURI

## Type Lists

* HTML5 Forms: <http://www.whatwg.org/specs/web-apps/current-work/#attr-input-type>

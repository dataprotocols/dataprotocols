---
title: Changelog
sidebar:
  order: 10
---

This document includes all meaningful changes made to the **Data Package standard**. It does not cover changes made to other documents like Recipes or Guides.

## v2.1

##### `schema.fieldsMatch` (fixed)

[fieldsMatch](/standard/table-schema/#fieldsMatch) has been corrected from array to string to match its definition ([#965](https://github.com/frictionlessdata/datapackage/issues/965)).

## v2.0

This release includes a rich set of specification improvements to make Data Package a finished product (see [announcement](https://frictionlessdata.io/blog/2023/11/15/frictionless-specs-update/)). All changes were reviewed and accepted by the Data Package Working Group.

> June 26, 2024

##### Tabular Data Package (removed)

The [Tabular Data Package](https://specs.frictionlessdata.io/tabular-data-package/) (`package.profile: "tabular-data-package"`) is removed. It does not add any benefits over defining `type: "table"` (previously `resource.profile: "tabular-data-resource"`) for its resources, which is more modular ([#52](https://github.com/frictionlessdata/datapackage-v2-draft/pull/52)).

##### `package.$schema` (new)

[`$schema`](/standard/data-package/#dollar-schema) replaces the `profile` property and allows easier extension and versioning ([#42](https://github.com/frictionlessdata/datapackage-v2-draft/pull/42)).

##### `package.contributors` (updated)

[`contributors`](/standard/data-package/#contributors) was updated:

- `contributor.title` is no longer required ([#7](https://github.com/frictionlessdata/datapackage-v2-draft/pull/7)).
- `contributor.givenName` and `contributor.familyName` are new properties to specify the given and family name of contributor, if it is a person ([#20](https://github.com/frictionlessdata/datapackage-v2-draft/pull/20)).
- `contributor.role` has been deprecated in favour of `contributor.roles`, see further ([#18](https://github.com/frictionlessdata/datapackage-v2-draft/pull/18)).
- `contributor.roles` is a new property that allows to specify multiple roles per contributor, rather than having to duplicate the contributor. It recommendeds to follow an established vocabulary and has suggested values that are different from the deprecated `contributor.role` ([#18](https://github.com/frictionlessdata/datapackage-v2-draft/pull/18)).

##### `package.version` (updated)

[`version`](/standard/data-package/#version) is now included in the specification, while in Data Package v1 it was erroneously only part of the documentation ([#3](https://github.com/frictionlessdata/datapackage-v2-draft/pull/3)).

##### `package.sources` (updated)

[`sources`](/standard/data-package/#sources) was updated:

- `source.title` is no longer required ([#7](https://github.com/frictionlessdata/datapackage-v2-draft/pull/7)).
- `source.version` is a new property to specify which version of a source was used ([#10](https://github.com/frictionlessdata/datapackage-v2-draft/pull/10)).

##### `resource.name` (updated)

[name](/standard/data-resource/#name) now allows any string. It previously required the name to only consist of lowercase alphanumeric characters plus `.`, `-` and `_`. The property is still required and must be unique among resources ([#27](https://github.com/frictionlessdata/datapackage-v2-draft/pull/27)).

##### `resource.path` (updated)

[path](/standard/data-resource/#path-or-data-required) now explicitely forbids hidden folders (starting with dot `.`) ([#19](https://github.com/frictionlessdata/datapackage-v2-draft/pull/19)).

##### `resource.type` (new)

[`type`](/standard/data-resource/#type) allows to specify the resource type ([#51](https://github.com/frictionlessdata/datapackage-v2-draft/pull/51)). `resource.type: "table"` replaces `resource.profile: "tabular-data-resource"`.

##### `resource.$schema` (new)

[`$schema`](/standard/data-resource/#dollar-schema) replaces the `profile` property and allows easier extension and versioning ([#42](https://github.com/frictionlessdata/datapackage-v2-draft/pull/42)). See also [resource.type](#resource-type-new).

##### `resource.encoding` (updated)

[encoding](/standard/data-resource/#encoding)'s definition has been updated to support binary formats like Parquet ([#15](https://github.com/frictionlessdata/datapackage-v2-draft/pull/15)).

##### `resource.sources` (updated)

[`sources`](/standard/data-resource/#sources) now inherits from a containing data package ([#57](https://github.com/frictionlessdata/datapackage-v2-draft/pull/57)).

##### Table Dialect (new)

[Table Dialect](/standard/table-dialect) is a new specification that superseeds and extends the CSV Dialect specification. It support other formats like JSON or Excel ([#41](https://github.com/frictionlessdata/datapackage-v2-draft/pull/41)).

##### `dialect.schema` (new)

[`schema`](/standard/table-dialect/#dollar-schema) allows extension and versioning ([#42](https://github.com/frictionlessdata/datapackage-v2-draft/pull/42)).

##### `dialect.table` (new)

[`table`](/standard/table-dialect/#table) allows to specify a table in a database ([#64](https://github.com/frictionlessdata/datapackage-v2-draft/pull/64)).

##### `schema.$schema` (new)

[`$schema`](/standard/table-schema/#dollar-schema) allows extension and versioning ([#42](https://github.com/frictionlessdata/datapackage-v2-draft/pull/42)).

##### `schema.fieldsMatch` (new)

[fieldsMatch](/standard/table-schema/#fieldsMatch) allows to specify how fields in a Table Schema match the fields in the data source. The default (`exact`) matches the Data Package v1 behaviour, but other values (e.g. `subset`, `superset`) allow to define fewer or more fields and match on field names. This new property extends and makes explicit the `schema_sync` option in Frictionless Framework ([#39](https://github.com/frictionlessdata/datapackage-v2-draft/pull/39)).

##### `schema.missingValues` (updated)

[`missingValues`](/standard/table-schema/#missingValues) now allow to specify labeled missingness ([#68](https://github.com/frictionlessdata/datapackage-v2-draft/pull/68)).

##### `schema.primaryKey` (updated)

[`primaryKey`](/standard/table-schema/#primaryKey) should now always be an array of strings, not a string ([#28](https://github.com/frictionlessdata/datapackage-v2-draft/pull/28)).

##### `schema.uniqueKeys` (new)

[`uniqueKeys`](/standard/table-schema/#uniqueKeys) allows to specify which fields are required to have unique logical values. It is an alternative to `field.contraints.unique` and is modelled after the corresponding SQL feature ([#30](https://github.com/frictionlessdata/datapackage-v2-draft/pull/30)).

##### `schema.foreignKeys` (updated)

[`foreignKeys`](/standard/table-schema/#foreignKeys) was updated:

- It should now always be an array of strings, not a string ([#28](https://github.com/frictionlessdata/datapackage-v2-draft/pull/28)).
- `foreignKeys.reference.resource` can now be omitted for self-referencing foreign keys. Previously it required setting `resource` to an empty string ([#29](https://github.com/frictionlessdata/datapackage-v2-draft/pull/29)).

##### `field.categories` (new)

[`categories`](/standard/table-schema/#categories) adds support for categorical data for the `string` and `integer` field types ([#68](https://github.com/frictionlessdata/datapackage-v2-draft/pull/68)).

##### `field.categoriesOrdered` (new)

[`categoriesOrdered`](/standard/table-schema/#categoriesOrdered) adds support for ordered categorical data for the `string` and `integer` field types ([#68](https://github.com/frictionlessdata/datapackage-v2-draft/pull/68)).

##### `field.missingValues` (new)

[`missingValues`](/standard/table-schema/#field-missingValues) allows to specify missing values per field, and overwrites `missingValues` specified at a resource level ([#24](https://github.com/frictionlessdata/datapackage-v2-draft/pull/24)).

##### `integer` field type (updated)

[`integer`](/standard/table-schema/#integer) now has a `groupChar` property. It was already available for `number` ([#6](https://github.com/frictionlessdata/datapackage-v2-draft/pull/6)).

##### `list` field type (new)

[`list`](/standard/table-schema/#list) allows to specify fields containing collections of primary values separated by a delimiter (e.g. `value1,value2`) ([#38](https://github.com/frictionlessdata/datapackage-v2-draft/pull/38)).

##### `datetime` field type (updated)

[`datetime`](/standard/table-schema/#datetime)'s default `format` is now extended to allow optional milliseconds and timezone parts ([#23](https://github.com/frictionlessdata/datapackage-v2-draft/pull/23)).

##### `geopoint` field type (updated)

[`geopoint`](/standard/table-schema/#geopoint)'s definition now clarifies that floating point numbers can be used for coordinate definitions ([#14](https://github.com/frictionlessdata/datapackage-v2-draft/pull/14)).

##### `any` field type (updated)

[`any`](/standard/table-schema/#any) is now the default field type and clarifies that the field type should not be inferred if not provided ([#13](https://github.com/frictionlessdata/datapackage-v2-draft/pull/13)).

##### `minimum` and `maximum` field constraints (updated)

[`minimum`](/standard/table-schema/#minimum) and [`maximum`](/standard/table-schema/#maximum) are now extended to support the `duration` field type ([#8](https://github.com/frictionlessdata/datapackage-v2-draft/pull/8)).

##### `exclusiveMinimum` and `exclusiveMaximum` field constraints (new)

[`exclusiveMinimum`](/standard/table-schema/#exclusiveMinimum) and [`exclusiveMaximum`](/standard/table-schema/#exclusiveMaximum) can be used to specify exclusive minimum and maximum values ([#11](https://github.com/frictionlessdata/datapackage-v2-draft/pull/11)).

##### `jsonschema` field constraint (new)

[`jsonSchema`](/standard/table-schema/#jsonSchema) can be used for the `object` and `array` field types ([#32](https://github.com/frictionlessdata/datapackage-v2-draft/pull/32)).

## v1.0

> September 5, 2017

Please refer to the [Data Package (v1) website](https://specs.frictionlessdata.io/).

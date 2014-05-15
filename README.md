# Hyperextend

Hyperextend is a library of components for extending media types. It's purpose is to provide a group of reusable hypermedia components that can be used to extend any hypermedia JSON document. Each component builds on top of the `Base` component. Additionally, the entire library is built on top of the [Hyperschema.org Core Definitions](http://hyperschema.org/core/).

While it is opinionated on how it groups the core definitions, it leaves the domain-specifics up to the API designer. Properties can be required, or made to include special rules for formating. Entire components or partial components can be used.

**Status**: Draft  
**Version**: 0.1.0

## Base

The base properties for any component.

### Schema

```json
{
  "title": "Base",
  "description": "The base object for Hyperextend",
  "type": "object",
  "properties": {
    "id": { "$ref": "http://hyperschema.org/core/base#/definitions/id" },
    "classes": { "$ref": "http://hyperschema.org/core/base#h/definitions/classes" },
    "title": { "$ref": "http://hyperschema.org/core/base#/definitions/title" },
    "description": { "$ref": "http://hyperschema.org/core/base#/definitions/description" },
    "version": { "$ref": "http://hyperschema.org/core/meta#/definitions/version" },
    "extends": {
      "title": "Extends URI",
      "description": "URI to JSON to extend",
      "type": "string"
    }
  }
}
```
### Properties

#### `id`

Unique identifier

#### `classes`

Classifier for a resource to give name or type

#### `title`

Human-readable title

#### `description`

Human-readable description

#### `extends`

URI for schema to extend

## Example

```json
{
  "id": "customer-34",
  "classes": [ "customer" ],
  "title": "Customer",
  "description": "Customer from CRM System"
}
```

## BaseLink

Extends `Base`

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/base#" },
    {
      "properties": {
        "prefixes": {
          "type": "array",
          "items": { "$ref": "#/definitions/prefix" }
        },
        "rels": { "$ref": "http://hyperschema.org/core/link#/definitions/rels" },
        "responseTypes": { "$ref": "http://hyperschema.org/core/link#/definitions/mediaTypes" },
        "typeOf": { "$ref": "http://hyperschema.org/core/meta#/definitions/typeOf" },
        "embedAs": { "type": "string" }
      },
      "definitions": {
        "prefix": { 
          "properties": {
            "prefix": { "$ref": "http://hyperschema.org/core/base#/definitions/prefix" },
            "href": { "$ref": "http://hyperschema.org/core/link#/definitions/href" }
          }
        }
      }
    }
  ]
}
```

### Properties

#### `prefixes`

Array of `prefix` objects

#### `prefix`

Prefix for use with curies

* `prefix` - Prefix (e.g. schema)
* `href` - URL (e.g. http://schema.org)

#### `rels`

Array of link relations.

#### `responseTypes`

Available media types that represent resource on server

#### `typeOf`

Type of the property (e.g. Schema.org)

#### `embedAs`

Instructs client on how to embed the resource/link (TBD)

### Example

```json
{
  "prefixes": [
    { "prefix": "schema", "href": "http://schema.org"}
  ],
  "rels": [ "item", "http://example.com/rels/customer"],
  "responseTypes": [
    "image/jpg",
    "image/gif"
  ],
  "typeOf": "schema:Person#image",
  "embedAs": "image"
}
```

## Field

Extends `Base`

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/base#" },
    {
      "properties": {
        "name": { "$ref": "http://hyperschema.org/core/fields#/definitions/name" },
        "defaultValue": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" },
        "currentValue": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" },
        "value": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" },
        "options": { "$ref": "http://hyperschema.org/core/fields#/definitions/options" },
        "type": { "$ref": "http://hyperschema.org/core/meta#/definitions/jsonType" },
        "format": { "$ref": "http://hyperschema.org/mediatypes/html#/definitions/type" },
        "label": { "$ref": "http://hyperschema.org/core/fields#/definitions/label" },
        "mapsTo": { "$ref": "http://hyperschema.org/core/fields#/definitions/mapsTo" }
      }
    }
  ]
}
```

### Properties

#### `name`

Name of field

#### `defaultValue`

Default value of field

#### `currentValue`

The current value of the field (usually returned from the server)

#### `value`

The value of the field (usually being sent to the server)

#### `options`

Field options (similar to select tag in HTML with options)

#### `type`

The type of the field from available JSON types

#### `format`

Format of field (from HTML types)

#### `label`

Human-readable label

#### `mapsTo`

Property to which this field corresponds

### Example

More examples below where this is used.

```json
{
  "name": "color",
  "defaultValue": "blue",
  "currentValue": "red",
  "options": [
    { "name": "Red", "value": "red" },
    { "name": "Blue", "value": "blue" },
    { "name": "Green", "value": "green" }
  ],
  "type": "string",
  "label": "Color",
  "mapsTo": "color-property"
}
```

## Link

Extends `BaseLink`

A link is considered to be safe GET requests.

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/baselink#" },
    {
      "properties": {
        "href": { "$ref": "http://hyperschema.org/core/link#/definitions/href" }
      }
    }
  ]
}
```

### Properties

#### `href`

URL for resource/link.

### Example

```json
{
  "id": "customer-4",
  "classes": [ "customer" ],
  "prefixes": [
    { "prefix": "schema", "href": "http://schema.org"}
  ],
  "rels": [ "item", "http://example.com/rels/customer"],
  "responseTypes": [
    "image/jpg",
    "image/gif"
  ],
  "typeOf": "schema:Person#image",
  "href": "/customer/4/image",
  "embedAs": "image"
}
```

## TemplatedLink

Extends `BaseLink`

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/baselink#" },
    {
      "properties": {
        "hreft": { "$ref": "http://hyperschema.org/core/link#/definitions/hrefTemplated" },
        "uriParams": { 
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extend/hyperextend/field#" }
        }
      }
    }
  ]
}
```

### Properties

#### `hreft`

URI template according to RFC 6570.

#### `uriParams`

Array of parameters for template

### Example

```json
{
  "id": "customer-4",
  "classes": [ "customer" ],
  "title": "Customer Picture",
  "prefixes": [
    { "prefix": "schema", "href": "http://schema.org"}
  ],
  "rels": [ "item", "http://example.com/rels/customer"],
  "responseTypes": [
    "image/jpg",
    "image/gif"
  ],
  "typeOf": "schema:Person#image",
  "hreft": "/customer/{id}/image",
  "embedAs": "image",
  "uriParams": [
    {
      "name": "id",
      "type": "number",
      "label": "Customer ID",
      "mapsTo": "id"
    }
  ]
}
```

## BaseQuery

### Schema

```json
{
  "properties": {
    "queryParams": {
      "type": "array",
      "items": { "$ref": "http://hyperschema.org/extend/hyperextend/field#" }
    } 
  }
}
```

### Properties

#### `queryParams`

Array of query parameters

## Query

Extends `Link` and `BaseQuery`

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/link#" },
    { "$ref": "http://hyperschema.org/extend/hyperextend/basequery#" }
  ]
}
```

### Example

```json
{
  "id": "customer-4",
  "classes": [ "customer" ],
  "title": "Customer Picture",
  "rels": [ "item", "http://example.com/rels/customer-images"],
  "responseTypes": [
    "image/jpg",
    "image/gif"
  ],
  "hreft": "/customer_images",
  "queryParams": [
    {
      "name": "id",
      "type": "number",
      "label": "Customer ID",
      "mapsTo": "id"
    }
  ]
}
```

## TemplatedQuery

Extends `TemplatedLink` and `BaseQuery`

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/templatedlink#" },
    { "$ref": "http://hyperschema.org/extend/hyperextend/basequery#" }
  ]
}
```

### Example

```json
{
  "id": "customer-4",
  "classes": [ "customer" ],
  "title": "Customer Images",
  "rels": [ "item", "http://example.com/rels/customer-images"],
  "responseTypes": [
    "image/jpg",
    "image/gif"
  ],
  "hreft": "/customer/{id}/images",
  "uriParams": [
    {
      "name": "id",
      "type": "number",
      "label": "Customer ID",
      "mapsTo": "id"
    }
  ],
  "queryParams": [
    {
      "name": "location",
      "type": "string",
      "label": "Image Location"
    }
  ]
}
```

## BaseAction

This is the only link type that can have a different HTTP method.

### Schema

```json
{
  "properties": {
    "method": { "$ref": "http://hyperschema.org/protocols/http#/definitions/method" },
    "bodyParams": {
      "type": "array",
      "items": { "$ref": "http://hyperschema.org/extend/hyperextend/field#" }
    } 
  }
}
```

## Action

Extends `Link` and `BaseAction`

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/link#" },
    { "$ref": "http://hyperschema.org/extend/hyperextend/baseaction#" }
  ]
}
```

### Example

```json
{
  "title": "Create Customer",
  "rels": [ "http://example.com/rels/customers"],
  "href": "/customers",
  "method": "POST",
  "bodyParams": [
    {
      "name": "first_name",
      "type": "string",
      "label": "First Name"
    },
    {
      "name": "last_name",
      "type": "string",
      "label": "Last Name"
    }
  ]
}
```

## TemplatedAction

Extends `TemplatedLink` and `BaseAction`

A way to have a URI template and Action in the same object.

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/templatedlink#" },
    { "$ref": "http://hyperschema.org/extend/hyperextend/baseaction#" }
  ]
}
```

### Example

```json
{
  "title": "Edit Customer",
  "rels": [ "http://example.com/rels/customers"],
  "hreft": "/customers/{id}",
  "method": "POST",
  "uriParams": [
    {
      "name": "id",
      "type": "number",
      "mapsTo": "id"
    }
  ],
  "bodyParams": [
    {
      "name": "first_name",
      "type": "string",
      "label": "First Name"
    },
    {
      "name": "last_name",
      "type": "string",
      "label": "Last Name"
    }
  ]
}
```

## Semantic

Extends `Base`

A semantic is a way to define semantics for properties.

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/base#" },
    {
      "properties": {
        "semantic": {
          "type": "object",
          "properties": {
            "name": { "$ref": "http://hyperschema.org/core/properties#/definitions/name" },
            "type": { "$ref": "http://hyperschema.org/core/meta#/definitions/jsonType" },
            "format": { "$ref": "http://hyperschema.org/mediatypes/html#/definitions/type" },
            "value": { "$ref": "http://hyperschema.org/core/properties#/definitions/value" },
            "typeOf": { "$ref": "http://hyperschema.org/core/meta#/definitions/typeOf" },
            "label": { "$ref": "http://hyperschema.org/core/properties#/definitions/label" }
          }
        }
      }
    }
  ]
}
```

### Properties

#### `name`

Name of the property

#### `type`

Type of the property (JSON types)

#### `format`

Format for the property (HTML.input types)

#### `value`

Value of the property

#### `typeOf`

Type of the property (e.g. Schema.org)

#### `label`

Human-readable label of the property

### Example

```json
{
  "name": "email",
  "type": "string",
  "format": "email",
  "value": "john@doe.com",
  "label": "Email"
}
```

## Resource Template

Extends `Base`

A resource template for adding and updating a resource.

### Schema

```json
{
  "allOf": [
    { "$ref": "http://hyperschema.org/extend/hyperextend/base#" },
    {
      "properties": {
        "mediaTypes": { "$ref": "http://hyperschema.org/core/link#/definitions/mediaTypes" },
        "fields": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extend/hyperextend/field#" }
        },
        "jsonSchema": { "$ref": "http://hyperschema.org/core/meta#/definitions/jsonSchema" }
      }
    }
  ]
}
```
### Properties

#### `mediaTypes`

Array of possible media types to use with the template

#### `fields`

Array of Hyperextend `Field` objects

#### `jsonSchema`

A JSON Schema object to define the schema of a possible JSON body.

### Example

```json
{
  "mediaTypes": [ "application/x-www-form-urlencoded" ],
  "fields": [
    {
      "name": "first_name",
      "type": "string",
      "label": "First Name"
    },
    {
      "name": "last_name",
      "type": "string",
      "label": "Last Name"
    }
  ]
}
```


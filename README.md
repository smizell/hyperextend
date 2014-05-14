# Hyperextend

Hyperextend is a library of components for extending media types. 

**Status**: Draft  
**Version**: 0.1.0  
**Proposed registration**: TBD

## Base

The base properties for any component.

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

### `id`

Unique identifier

### `classes`

Classifier for a resource to give name or type

### `title`

Human-readable title

### `description`

Human-readable description

### `extends`

URI for schema to extend

## BaseLink

Extends `Base`

```json
{
  "properties": {
    "prefixes": {
      "type": "array",
      "items": { "$ref": "#/definitions/prefix" }
    },
    "rels": { "$ref": "http://hyperschema.org/core/link#/definitions/rels" },
    "responseTypes": { "$ref": "http://hyperschema.org/core/link#/definitions/mediaTypes" },
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
```

### `prefixes`

Array of `prefix` objects

### `prefix`

Prefix for use with curies

* `prefix` - Prefix (e.g. schema)
* `href` - URL (e.g. http://schema.org)

### `rels`

Array of link relations.

### `responseTypes`

Available media types that represent resource on server

### `availableMethods`

Available HTTP methods for resource/link

### `embedAs`

Instructs client on how to embed the resource/link (TBD)

## Field

Extends `Base`

```json
{
  "properties": {
    "name": { "$ref": "http://hyperschema.org/core/fields#/definitions/name" },
    "defaultValue": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" },
    "currentValue": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" },
    "options": { "$ref": "http://hyperschema.org/core/fields#/definitions/options" },
    "type": { "$ref": "http://hyperschema.org/mediatypes/html#/definitions/type" },
    "label": { "$ref": "http://hyperschema.org/core/fields#/definitions/label" },
    "mapsTo": { "$ref": "http://hyperschema.org/core/fields#/definitions/mapsTo" }
  }
}
```

### `currentValue`

The current value of the field

## Link

Extends `BaseLink`

A link is considered to be safe GET requests.

```json
{
  "properties": {
    "href": { "$ref": "http://hyperschema.org/core/link#/definitions/href" }
  }
}
```

### `href`

URL for resource/link.

## BaseTemplatedLink

Extends `BaseLink`

```json
{
  "properties": {
    "hreft": { "$ref": "http://hyperschema.org/core/link#/definitions/hrefTemplated" },
    "uriParams": { 
      "type": "array",
      "items": { "$ref": "http://hyperschema.org/extend/hyperextend/field#" }
    }
  }
}
```

### `hreft`

URI template according to RFC 6570.

### `uriParams`

Array of parameters for template

## TemplatedLink

Extends `BaseTemplatedLink`

Should be a `GET` method.

## Query

Extends `Link`

Analogous with HTML form with method set to `GET`.

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

### `queryParams`

Array of query parameters

## TemplatedQuery

Extends `BaseTemplatedLink`

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

## Action

Extends `BaseLink`

This is the only link type that can have a different HTTP method.

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

## TemplatedAction

Extends `Action` and `BaseTemplatedLink`

A way to have a URI template and Action in the same object.

## Semantic

Extends `Base`

A semantic is a way to define semantics for properties

```json
{
  "properties": {
    "semantic": {
      "type": "object",
      "properties": {
        "name": { "$ref": "http://hyperschema.org/core/properties#/definitions/name" },
        "type": { "$ref": "http://hyperschema.org/core/meta#/definitions/jsonType" }
        "format": { "$ref": "http://hyperschema.org/mediatypes/html#/definitions/type" },
        "value": { "$ref": "http://hyperschema.org/core/properties#/definitions/value" },
        "typeOf": { "$ref": "http://hyperschema.org/core/meta#/definitions/typeOf" },
        "label": { "$ref": "http://hyperschema.org/core/properties#/definitions/label" }
      }
    }
  }
}
```

### `name`

Name of the property

### `type`

Type of the property (JSON types)

### `format`

Format for the property (HTML.input types)

### `value`

Value of the property

### `typeOf`

Type of the property (e.g. Schema.org)

### `label`

Human-readable label of the property

## Template

Extends `Base`

A template is way to provide a resource template.

```json
{
  "properties": {
    "template": {
      "properties": {
        "mediaTypes": { "$ref": "http://hyperschema.org/core/link#/definitions/mediaTypes" },
        "fields": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extend/hyperextend/field#" }
        },
        "jsonSchema": { "$ref": "http://hyperschema.org/core/meta#/definitions/jsonSchema" }
      }
    }
  }
}
```

### `mediaTypes`

Array of possible media types to use with the template

### `fields`

Array of Hyperextend `Field` objects

### `jsonSchema`

A JSON Schema object to define the schema of a possible JSON body.


# Hyperextend

Hyperextend is a library of components for extending media types. 

**Status**: Draft  
**Version**: 0.1.0

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
    "availableMethods": { "$ref": "http://hyperschema.org/protocols/http#/definitions/methods" },
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

Array of link relations. REQUIRED

### `responseTypes`

Available media types that represent resource on server

### `availableMethods`

Available HTTP methods for resource/link

### `embedAs`

Instructs client on how to embed the resource/link (TBD)

## BaseField

Extends `Base`

```json
{
  "properties": {
    "name": { "$ref": "http://hyperschema.org/core/fields#/definitions/name" },
    "defaultValue": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" },
    "currentValue": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" },
    "options": {
      "type": "array",
      "items": { "$ref": "#/definitions/option" }
    },
    "type": { "$ref": "http://hyperschema.org/mediatypes/html#/definitions/type" },
    "label" { "$ref": "http://hyperschema.org/core/fields#/definitions/label" },
    "mapsTo": { "$ref": "http://hyperschema.org/core/fields#/definitions/mapsTo" }
  },
  "definitions": {
    "option": {
      "properties": {
        "name": { "$ref": "http://hyperschema.org/core/fields#/definitions/name" },
        "value": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" }
      }
    }
  }
}
```

## Field

Extends `BaseField`

```json
{
  "properties": {
    "currentValue": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" },
  }
}
```

### `currentValue`

The current value of the field

## Link

Extends `BaseLink`

```json
{
  "properties": {
    "href": { "$ref": "http://hyperschema.org/core/link#/definitions/href" }
  }
}
```

### `href`

URL for resource/link. REQUIRED

## TemplatedLink

Extends `BaseLink`

```json
{
  "properties": {
    "hreft": { "$ref": "http://hyperschema.org/core/link#/definitions/hrefTemplated" },
    "params": { 
      "type": "array",
      "items": { "$ref": "http://hyperschema.org/extend/hyperextend/field# }
    }
  }
}
```

### `hreft`

URI template according to RFC 6570.

### `params`

Array of parameters for template

## Query

Extends `Link` or `TemplatedLink`

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

## BaseResource

Extends `Link`

```json
{
  "properties": {
    "semantics": {
      "type": "array",
      "items": { "$ref": "#/definitions/semantic" }
    },
    "semantic": {
      "properties": {
        "name": { "$ref": "http://hyperschema.org/core/properties#/definitions/name" },
        "type": { "$ref": "http://hyperschema.org/mediatypes/html#/definitions/type" },
        "value": { "$ref": "http://hyperschema.org/core/properties#/definitions/value" },
        "typeOf": { "$ref": "http://hyperschema.org/core/meta#/definitions/typeOf" },
        "label": { "$ref": "http://hyperschema.org/core/properties#/definitions/label" }
      }
    },
    "templates": {
      "type": "array",
      "items": { "$ref": "#/definitions/template" }
    }
  },
  "definitions": {
    "template": {
      "properties": {
        "mediaType": { "$ref": "http://hyperschema.org/core/link#/definitions/mediaType" },
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

### `semantics`

Array of `semantic` objects

### `semantic`

Semantic for properity

* `name`
* `type`
* `value`
* `typeOf`
* `label`

### Properties

Properites of resource

### `templates`

Array of template objects

### `template`

Template for different media type

* `mediaType` - Specifies the media type
* `fields` - Specifies fields, if applicable
* `jsonSchema` - Specifies JSON Schema, if applicable

## Resource

Extends `BaseResource`

```json
{
  "properties": {
    "properties": { "$ref": "http://hyperschema.org/core/properties#/definitions/propertyObject" },
    "links": {
      "type": "array",
      "item": { "$ref": "http://hyperschema.org/extend/hyperextend/link#" }
    },
    "linkTemplates": {
      "type": "array",
      "item": { "$ref": "http://hyperschema.org/extend/hyperextend/linkTemplate#" }
    },
    "queries": {
      "type": "array",
      "item": { "$ref": "http://hyperschema.org/extend/hyperextend/query#" }
    },
    "resources": {
      "type": "array",
      "item": { "$ref": "http://hyperschema.org/extend/hyperextend/resource#" }
    }
  }
}
```

### `links`

Array of Hyperextend `BaseLink` objects

### `linkTemplates`

Array of Hyperextend `TemplateLink` objects

### `queries`

Array of Hyperextend `Query` objects

### `resources`

Array of Hyperextend `FullResource` objects

## Hyperextend

```json
{
  "type": "object",
  "properities": {
    "hyperextend": {
      "oneOf": [
        { "$ref": "http://hyperschema.org/extend/hyperextend/link#" },
        { "$ref": "http://hyperschema.org/extend/hyperextend/templatedLink#" },
        { "$ref": "http://hyperschema.org/extend/hyperextend/query#" },
        { "$ref": "http://hyperschema.org/extend/hyperextend/resource#" }
      ]
    }
  }
}
```

### `hyperextend`

Main hyperextend object, which can be a link, templated link, query, or resource
# Hyperextend

Hyperextend is a library of components for extending media types. 

## Base

The base properties for any component.

```json
{
  "definitions": {
    "id": { "$ref": "http://hyperschema.org/core/base#/definitions/id" },
    "classes": { "$ref": "http://hyperschema.org/core/base#h/definitions/classes" },
    "title": { "$ref": "http://hyperschema.org/core/base#/definitions/title" },
    "description": { "$ref": "http://hyperschema.org/core/base#/definitions/description" },
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
  "definitions": {
    "prefixes": {
      "type": "array",
      "items": { "$ref": "#/definitions/prefix" }
    },
    "prefix": { 
      "properties": {
        "prefix": { "$ref": "http://hyperschema.org/core/base#/definitions/prefix" },
        "href": { "$ref": "http://hyperschema.org/core/link#/definitions/href" }
      }
    },
    "rels": { "$ref": "http://hyperschema.org/core/link#/definitions/rels" },
    "href": { "$ref": "http://hyperschema.org/core/link#/definitions/href" },
    "responseTypes": { "$ref": "http://hyperschema.org/core/link#/definitions/mediaTypes" },
    "availableMethods": { "$ref": "http://hyperschema.org/protocols/http#/definitions/methods" },
    "embedAs": { "type": "string" }
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

### `href`

URL for resource/link. REQUIRED

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
  "definitions": {
    "field": {
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
      }
    },
    "option": {
      "properties": {
        "name": { "$ref": "http://hyperschema.org/core/fields#/definitions/name" },
        "value": { "$ref": "http://hyperschema.org/core/fields#/definitions/value" }
      }
    }
  }
}
```

## TemplatedLink

Extends `BaseLink`

```json
{
  "definitions": {
    "hreft": { "$ref": "http://hyperschema.org/core/link#/definitions/hrefTemplated" },
    "params": { 
      "type": "array",
      "items": { "$ref": "#/definitions/param" }
    },
    "param": { "$ref": "http://hyperschema.org/extend/hyperextend/field#/definitions/field" }
  }
}
```

### `hreft`

URI template according to RFC 6570.

### `params`

Array of parameters for template

### `param`

Describes available paramaters in template

## Query

Analogous with HTML form with method set to `GET`.

Extends `BaseLink`

```json
{
  "definitions": {
    "queryParams": {
      "type": "array",
      "items": { "$ref": "#/definitions/param" }
    },
    "param": { "$ref": "http://hyperschema.org/extend/hyperextend/field#/definitions/field" }
  }
}
```

### `queryParams`

Array of query parameters

### `param`

Query param

## BaseResource

Extends `BaseLink`

```json
{
  "definitions": {
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
    "properties": { "$ref": "http://hyperschema.org/core/properties#/definitions/propertyObject" },
    "templates": {
      "type": "array",
      "items": { "$ref": "#/definitions/template" }
    },
    "template": {
      "properties": {
        "mediaType": { "$ref": "http://hyperschema.org/core/link#/definitions/mediaType" },
        "fields": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extend/hyperextend/field#/definitions/field" }
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

## Instance Schemeas

These include data that is specific to instances

## FullResource

Extends `BaseResource`

```json
{
  "definitions": {
    "properties": {
      "links": {
        "type": "array",
        "item": { "$ref": "http://hyperschema.org/extend/hyperextend/link#/definitions/link" }
      },
      "linkTemplates": {
        "type": "array",
        "item": { "$ref": "http://hyperschema.org/extend/hyperextend/linkTemplate#/definitions/linkTemplate" }
      },
      "queries": {
        "type": "array",
        "item": { "$ref": "http://hyperschema.org/extend/hyperextend/query#/definitions/query" }
      },
      "resources": {
        "type": "array",
        "item": { "$ref": "http://hyperschema.org/extend/hyperextend/resource#/definitions/resource" }
      }
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

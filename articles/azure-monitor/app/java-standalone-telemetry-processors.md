---
title: Telemetry processors (preview) - Azure Monitor Application Insights for Java
description: Learn to configure telemetry processors in Azure Monitor Application Insights for Java.
ms.topic: conceptual
ms.date: 10/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
---

# Telemetry processors (preview) - Azure Monitor Application Insights for Java

> [!NOTE]
> The telemetry processors feature is in preview.

Application Insights Java 3.x can process telemetry data before the data is exported.

Here are some use cases for telemetry processors:
 * Mask sensitive data.
 * Conditionally add custom dimensions.
 * Update the span name, which is used to aggregate similar telemetry in the Azure portal.
 * Drop specific span attribute(s) to control ingestion costs.
 * Filter out some metrics to control ingestion costs.

> [!NOTE]
> If you are looking to drop specific (whole) spans for controlling ingestion cost,
> see [sampling overrides](./java-standalone-sampling-overrides.md).

## Terminology

Before you learn about telemetry processors, you should understand the terms *span* and *log*.

A span is a type of telemetry that represent one of:

* An incoming request.
* An outgoing dependency (for example, a remote call to another service).
* An in-process dependency (for example, work being done by subcomponents of the service).

A log is a type of telemetry that represents:

* log data captured from log4j, logback, and java.util.logging 

For telemetry processors, these span/log components are important:

* Name
* Body
* Attributes

The span name is the primary display for requests and dependencies in the Azure portal. Span attributes represent both standard and custom properties of a given request or dependency.

The trace message or body is the primary display for logs in the Azure portal. Log attributes represent both standard and custom properties of a given log

## Telemetry processor types

Currently, the four types of telemetry processors are attribute processors, span processors, log processors, and metric filters.

An attribute processor can insert, update, delete, or hash attributes of a telemetry item (`span` or `log`).
It can also use a regular expression to extract one or more new attributes from an existing attribute.

A span processor can update the telemetry name of requests and dependencies.
It can also use a regular expression to extract one or more new attributes from the span name.

A log processor can update the telemetry name of logs.
It can also use a regular expression to extract one or more new attributes from the log name.

A metric filter can filter out metrics to help control ingestion cost.

> [!NOTE]
> Currently, telemetry processors process only attributes of type string. They don't process attributes of type Boolean or number.

## Getting started

To begin, create a configuration file named *applicationinsights.json*. Save it in the same directory as *applicationinsights-agent-\*.jar*. Use the following template.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        ...
      },
      {
        "type": "attribute",
        ...
      },
      {
        "type": "span",
        ...
      },
      {
        "type": "log",
        ...
      },
      {
        "type": "metric-filter",
        ...
      }
    ]
  }
}
```

## Attribute processor

The attribute processor modifies attributes of a `span` or a `log`. It can support the ability to include or exclude `span` or `log`. It takes a list of actions that are performed in the order that the configuration file specifies. The processor supports these actions:

- `insert`
- `update`
- `delete`
- `hash`
- `extract`

### `insert`

The `insert` action inserts a new attribute in telemetry item where the `key` doesn't already exist.   

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "value1",
        "action": "insert"
      }
    ]
  }
]
```
The `insert` action requires the following settings:
* `key`
* Either `value` or `fromAttribute`
* `action`: `insert`

### `update`

The `update` action updates an attribute in telemetry item where the `key` already exists.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "newValue",
        "action": "update"
      }
    ]
  }
]
```
The `update` action requires the following settings:
* `key`
* Either `value` or `fromAttribute`
* `action`: `update`


### `delete` 

The `delete` action deletes an attribute from a telemetry item.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "delete"
      }
    ]
  }
]
```
The `delete` action requires the following settings:
* `key`
* `action`: `delete`

### `hash`

The `hash` action hashes (SHA1) an existing attribute value.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "hash"
      }
    ]
  }
]
```
The `hash` action requires the following settings:
* `key`
* `action`: `hash`

### `extract`

> [!NOTE]
> The `extract` feature is available only in version 3.0.2 and later.

The `extract` action extracts values by using a regular expression rule from the input key to target keys that the rule specifies. If a target key already exists, it's overridden. This action behaves like the [span processor](#extract-attributes-from-the-span-name) `toAttributes` setting, where the existing attribute is the source.

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "pattern": "<regular pattern with named matchers>",
        "action": "extract"
      }
    ]
  }
]
```
The `extract` action requires the following settings:
* `key`
* `pattern`
* `action`: `extract`

### Include criteria and exclude criteria

Attribute processors support optional `include` and `exclude` criteria.
A attribute processor is applied only to telemetry that match its `include` criteria (if it's provided)
_and_ don't match its `exclude` criteria (if it's provided).

To configure this option, under `include` or `exclude` (or both), specify at least one `matchType` and either `spanNames` or `attributes`.
The include-exclude configuration allows more than one specified condition.
All specified conditions must evaluate to true to result in a match. 

* **Required field**: `matchType` controls how items in `spanNames` arrays and `attributes` arrays are interpreted. Possible values are `regexp` and `strict`. 

* **Optional fields**: 
    * `spanNames` must match at least one of the items. 
    * `attributes` specifies the list of attributes to match. All of these attributes must match exactly to result in a match.
    
> [!NOTE]
> If both `include` and `exclude` are specified, the `include` properties are checked before the `exclude` properties are checked.

> [!NOTE]
> If the `include` or `exclude` configuration do not have `spanNames` specified, then the matching criteria is applied on both `spans` and `logs`.

### Sample usage

```json
"processors": [
  {
    "type": "attribute",
    "include": {
      "matchType": "strict",
      "spanNames": [
        "spanA",
        "spanB"
      ]
    },
    "exclude": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "redact_trace",
          "value": "false"
        }
      ]
    },
    "actions": [
      {
        "key": "credit_card",
        "action": "delete"
      },
      {
        "key": "duplicate_key",
        "action": "delete"
      }
    ]
  }
]
```
For more information, see [Telemetry processor examples](./java-standalone-telemetry-processors-examples.md).

## Span processor

The span processor modifies either the span name or attributes of a span based on the span name. It can support the ability to include or exclude spans.

### Name a span

The `name` section requires the `fromAttributes` setting. The values from these attributes are used to create a new name, concatenated in the order that the configuration specifies. The processor will change the span name only if all of these attributes are present on the span.

The `separator` setting is optional. This setting is a string. It's specified to split values.
> [!NOTE]
> If renaming relies on the attributes processor to modify attributes, ensure the span processor is specified after the attributes processor in the pipeline specification.

```json
"processors": [
  {
    "type": "span",
    "name": {
      "fromAttributes": [
        "attributeKey1",
        "attributeKey2",
      ],
      "separator": "::"
    }
  }
] 
```

### Extract attributes from the span name

The `toAttributes` section lists the regular expressions to match the span name against. It extracts attributes based on subexpressions.

The `rules` setting is required. This setting lists the rules that are used to extract attribute values from the span name. 

The values in the span name are replaced by extracted attribute names. Each rule in the list is a regular expression (regex) pattern string. 

Here's how values are replaced by extracted attribute names:

1. The span name is checked against the regex. 
2. If the regex matches, all named subexpressions of the regex are extracted as attributes. 
3. The extracted attributes are added to the span. 
4. Each subexpression name becomes an attribute name. 
5. The subexpression matched portion becomes the attribute value. 
6. The matched portion in the span name is replaced by the extracted attribute name. If the attributes already exist in the span, they're overwritten. 
 
This process is repeated for all rules in the order they're specified. Each subsequent rule works on the span name that's the output of the previous rule.

```json
"processors": [
  {
    "type": "span",
    "name": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]

```

## Common span attributes

This section lists some common span attributes that telemetry processors can use.

### HTTP spans

| Attribute  | Type | Description | 
|---|---|---|
| `http.method` | string | HTTP request method.|
| `http.url` | string | Full HTTP request URL in the form `scheme://host[:port]/path?query[#fragment]`. The fragment isn't usually transmitted over HTTP. But if the fragment is known, it should be included.|
| `http.status_code` | number | [HTTP response status code](https://tools.ietf.org/html/rfc7231#section-6).|
| `http.flavor` | string | Type of HTTP protocol. |
| `http.user_agent` | string | Value of the [HTTP User-Agent](https://tools.ietf.org/html/rfc7231#section-5.5.3) header sent by the client. |

### JDBC spans

| Attribute  | Type | Description  |
|---|---|---|
| `db.system` | string | Identifier for the database management system (DBMS) product being used. |
| `db.connection_string` | string | Connection string used to connect to the database. It's recommended to remove embedded credentials.|
| `db.user` | string | Username for accessing the database. |
| `db.name` | string | String used to report the name of the database being accessed. For commands that switch the database, this string should be set to the target database, even if the command fails.|
| `db.statement` | string | Database statement that's being run.|

### Include criteria and exclude criteria

Span processors support optional `include` and `exclude` criteria.
A span processor is applied only to telemetry that match its `include` criteria (if it's provided)
_and_ don't match its `exclude` criteria (if it's provided).

To configure this option, under `include` or `exclude` (or both), specify at least one `matchType` and either `spanNames` or  span `attributes`.
The include-exclude configuration allows more than one specified condition.
All specified conditions must evaluate to true to result in a match. 

* **Required field**: `matchType` controls how items in `spanNames` arrays and `attributes` arrays are interpreted. Possible values are `regexp` and `strict`. 

* **Optional fields**: 
    * `spanNames` must match at least one of the items. 
    * `attributes` specifies the list of attributes to match. All of these attributes must match exactly to result in a match.
    
> [!NOTE]
> If both `include` and `exclude` are specified, the `include` properties are checked before the `exclude` properties are checked.

### Sample usage

```json
"processors": [
  {
    "type": "span",
    "include": {
      "matchType": "strict",
      "spanNames": [
        "spanA",
        "spanB"
      ]
    },
    "exclude": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "attribute1",
          "value": "attributeValue1"
        }
      ]
    },
    "name": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]
```
For more information, see [Telemetry processor examples](./java-standalone-telemetry-processors-examples.md).

## Log processor

> [!NOTE]
> Log processors are available starting from version 3.1.1.

The log processor modifies either the log message body or attributes of a log based on the log message body. It can support the ability to include or exclude logs.

### Update Log message body

The `body` section requires the `fromAttributes` setting. The values from these attributes are used to create a new body, concatenated in the order that the configuration specifies. The processor will change the log body only if all of these attributes are present on the log.

The `separator` setting is optional. This setting is a string. It's specified to split values.
> [!NOTE]
> If renaming relies on the attributes processor to modify attributes, ensure the log processor is specified after the attributes processor in the pipeline specification.

```json
"processors": [
  {
    "type": "log",
    "body": {
      "fromAttributes": [
        "attributeKey1",
        "attributeKey2",
      ],
      "separator": "::"
    }
  }
] 
```

### Extract attributes from the log message body

The `toAttributes` section lists the regular expressions to match the log message body. It extracts attributes based on subexpressions.

The `rules` setting is required. This setting lists the rules that are used to extract attribute values from the body. 

The values in the log message body are replaced by extracted attribute names. Each rule in the list is a regular expression (regex) pattern string. 

Here's how values are replaced by extracted attribute names:

1. The log message body is checked against the regex. 
2. If the regex matches, all named subexpressions of the regex are extracted as attributes. 
3. The extracted attributes are added to the log. 
4. Each subexpression name becomes an attribute name. 
5. The subexpression matched portion becomes the attribute value. 
6. The matched portion in the log name is replaced by the extracted attribute name. If the attributes already exist in the log, they're overwritten. 
 
This process is repeated for all rules in the order they're specified. Each subsequent rule works on the log name that's the output of the previous rule.

```json
"processors": [
  {
    "type": "log",
    "body": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]

```

### Include criteria and exclude criteria

Log processors support optional `include` and `exclude` criteria.
A log processor is applied only to telemetry that match its `include` criteria (if it's provided)
_and_ don't match its `exclude` criteria (if it's provided).

To configure this option, under `include` or `exclude` (or both), specify the `matchType` and `attributes`.
The include-exclude configuration allows more than one specified condition.
All specified conditions must evaluate to true to result in a match. 

* **Required field**: 
  * `matchType` controls how items in `attributes` arrays are interpreted. Possible values are `regexp` and `strict`. 
  * `attributes` specifies the list of attributes to match. All of these attributes must match exactly to result in a match.
    
> [!NOTE]
> If both `include` and `exclude` are specified, the `include` properties are checked before the `exclude` properties are checked.

> [!NOTE]
> Log processors do not support `spanNames`.

### Sample usage

```json
"processors": [
  {
    "type": "log",
    "include": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "attribute1",
          "value": "value1"
        }
      ]
    },
    "exclude": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "attribute2",
          "value": "value2"
        }
      ]
    },
    "body": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]
```
For more information, see [Telemetry processor examples](./java-standalone-telemetry-processors-examples.md).

## Metric filter

> [!NOTE]
> Metric filters are available starting from version 3.1.1.

Metric filter are used to exclude some metrics in order to help control ingestion cost.

Metric filters only support `exclude` criteria. Metrics that match its `exclude` criteria will not be exported.

To configure this option, under `exclude`, specify the `matchType` one or more `metricNames`.

* **Required field**:
  * `matchType` controls how items in `metricNames` are matched. Possible values are `regexp` and `strict`.
  * `metricNames` must match at least one of the items.

### Sample usage

```json
"processors": [
  {
    "type": "metric-filter",
    "exclude": {
      "matchType": "strict",
      "metricNames": [
        "metricA",
        "metricB"
      ]
    }
  }
]
```

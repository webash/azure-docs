---
# Mandatory fields.
title: DTDL models
titleSuffix: Azure Digital Twins
description: Understand how Azure Digital Twins uses custom models to describe entities in your environment.
author: baanders
ms.author: baanders # Microsoft employees only
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins

# Optional fields. Don't forget to remove # if you need a field.
# ms.custom: can-be-multiple-comma-separated
# ms.reviewer: MSFT-alias-of-reviewer
# manager: MSFT-alias-of-manager-or-PM-counterpart
---

# Understand twin models in Azure Digital Twins

A key characteristic of Azure Digital Twins is the ability to define your own vocabulary and build your twin graph in the self-defined terms of your business. This capability is provided through user-provided **models**. You can think of models as the nouns in a description of your world. 

A model is similar to a **class** in an object-oriented programming language, defining a data shape for one particular concept in your real work environment. Models have names (such as *Room* or *TemperatureSensor*), and contain elements such as properties, telemetry/events, and commands that describe what this type of entity in your environment can do. Later, you will use these models to create [digital twins](concepts-twins-graph.md) that represent specific entities that meet this type description.

Azure Digital Twins models are represented in the JSON-LD-based **Digital Twin Definition Language (DTDL)**.  

## Digital Twin Definition Language (DTDL) for models

Models for Azure Digital Twins are defined using the Digital Twins Definition Language (DTDL). 

You can view the full language specs for DTDL in GitHub: [Digital Twins Definition Language (DTDL) - Version 2](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md).

DTDL is based on JSON-LD and is programming-language independent. DTDL is not exclusive to Azure Digital Twins, but is also used to represent device data in other IoT services such as [IoT Plug and Play](../iot-pnp/overview-iot-plug-and-play.md). Azure Digital Twins uses DTDL **version 2** (use of DTDL version 1 with Azure Digital Twins has now been deprecated). 

The rest of this article summarizes how the language is used in Azure Digital Twins.

### Azure Digital Twins DTDL implementation specifics

Not all services that use DTDL implement the exact same features of DTDL. For example, IoT Plug and Play does not use the DTDL features that are for graphs, while Azure Digital Twins does not currently implement DTDL commands. 

For a DTDL model to be compatible with Azure Digital Twins, it must meet these requirements:

* All top-level DTDL elements in a model must be of type *interface*. This is because Azure Digital Twins model APIs can receive JSON objects that represent either an interface or an array of interfaces. As a result, no other DTDL element types are allowed at the top level.
* DTDL for Azure Digital Twins must not define any *commands*.
* Azure Digital Twins only allows a single level of component nesting. This means that an interface that's being used as a component can't have any components itself. 
* Interfaces can't be defined inline within other DTDL interfaces; they must be defined as separate top-level entities with their own IDs. Then, when another interface wants to include that interface as a component or through inheritance, it can reference its ID.

Azure Digital Twins also does not observe the `writable` attribute on properties or relationships. Although this can be set as per DTDL specifications, the value isn't used by Azure Digital Twins. Instead, these are always treated as writable by external clients that have general write permissions to the Azure Digital Twins service.

## Model overview

### Elements of a model

Within a model definition, the top-level code item is an **interface**. This encapsulates the entire model, and the rest of the model is defined within the interface. 

A DTDL model interface may contain zero, one, or many of each of the following fields:
* **Property** - Properties are data fields that represent the state of an entity (like the properties in many object-oriented programming languages). Properties have backing storage and can be read at any time. For more information, see [Properties and telemetry](#properties-and-telemetry) below.
* **Telemetry** - Telemetry fields represent measurements or events, and are often used to describe device sensor readings. Unlike properties, telemetry is not stored on a digital twin; it is a series of time-bound data events that need to be handled as they occur. For more information, see [Properties and telemetry](#properties-and-telemetry) below.
* **Relationship** - Relationships let you represent how a digital twin can be involved with other digital twins. Relationships can represent different semantic meanings, such as *contains* ("floor contains room"), *cools* ("hvac cools room"), *isBilledTo* ("compressor is billed to user"), etc. Relationships allow the solution to provide a graph of interrelated entities. Relationships can also have properties of their own. For more information, see [Relationships](#relationships) below.
* **Component** - Components allow you to build your model interface as an assembly of other interfaces, if you want. An example of a component is a *frontCamera* interface (and another component interface *backCamera*) that are used in defining a model for a *phone*. You must first define an interface for *frontCamera* as though it were its own model, and then you can reference it when defining *Phone*.

    Use a component to describe something that is an integral part of your solution but doesn't need a separate identity, and doesn't need to be created, deleted, or rearranged in the twin graph independently. If you want entities to have independent existences in the twin graph, represent them as separate digital twins of different models, connected by **relationships**.
    
    >[!TIP] 
    >Components can also be used for organization, to group sets of related properties within a model interface. In this situation, you can think of each component as a namespace or "folder" inside the interface.

    For more information, see [Components](#components) below.


> [!NOTE]
> The [spec for DTDL](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) also defines **Commands**, which are methods that can be executed on a digital twin (like a reset command, or a command to switch a fan on or off). However, *commands are not currently supported in Azure Digital Twins.*

### Model code

Twin type models can be written in any text editor. The DTDL language follows JSON syntax, so you should store models with the extension .json. Using the JSON extension will enable many programming text editors to provide basic syntax checking and highlighting for your DTDL documents. There is also a [DTDL extension](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl) available for [Visual Studio Code](https://code.visualstudio.com/).

The fields of the model are:

| Field | Description |
| --- | --- |
| `@id` | An identifier for the model. Must be in the format `dtmi:<domain>:<unique-model-identifier>;<model-version-number>`. |
| `@type` | Identifies the kind of information being described. For an interface, the type is *Interface*. |
| `@context` | Sets the [context](https://niem.github.io/json/reference/json-ld/context/) for the JSON document. Models should use `dtmi:dtdl:context;2`. |
| `displayName` | [optional] Allows you to give the model a friendly name if desired. |
| `contents` | All remaining interface data is placed here, as an array of attribute definitions. Each attribute must provide a `@type` (**property**, **telemetry**, **command**, **relationship**, or **component**) to identify the sort of interface information it describes, and then a set of properties that define the actual attribute (for example, `name` and `schema` to define a **property**). |

#### Example model

This section contains an example of a basic model, written as a DTDL interface. 

This model describes a Home, with one **property** for an ID. The Home model also defines a **relationship** to a Floor model, which can be used to indicate that a Home twin is connected to certain Floor twins.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/basic-home-example/IHome.json":::

## Properties and telemetry

This section goes into more detail about **properties** and **telemetry** in DTDL models.

### Difference between properties and telemetry

Here's some additional guidance on conceptually distinguishing between DTDL **property** and **telemetry** in Azure Digital Twins.
* **Properties** are expected to have backing storage. This means that you can read a property at any time and retrieve its value. If the property is writable, you can also store a value in the property.  
* **Telemetry** is more like a stream of events; it's a set of data messages that have short lifespans. If you don't set up listening for the event and actions to take when it happens, there is no trace of the event at a later time. You can't come back to it and read it later. 
  - In C# terms, telemetry is like a C# event. 
  - In IoT terms, telemetry is typically a single measurement sent by a device.

**Telemetry** is often used with IoT devices, because many devices are not capable of, or interested in, storing the measurement values they generate. They just send them out as a stream of "telemetry" events. In this case, you can't inquire on the device at any time for the latest value of the telemetry field. Instead, you'll need to listen to the messages from the device and take actions as the messages arrive. 

As a result, when designing a model in Azure Digital Twins, you will probably use **properties** in most cases to model your twins. This allows you to have the backing storage and the ability to read and query the data fields.

Telemetry and properties often work together to handle data ingress from devices. As all ingress to Azure Digital Twins is via [APIs](concepts-apis-sdks.md), you will typically use your ingress function to read telemetry or property events from devices, and set a property in Azure Digital Twins in response. 

You can also publish a telemetry event from the Azure Digital Twins API. As with other telemetry, that is a short-lived event that requires a listener to handle.

### Schema

As per DTDL, the schema for **property** and **telemetry** attributes can be of standard primitive types—`integer`, `double`, `string`, and `Boolean`—and other types such as `DateTime` and `Duration`. 

In addition to primitive types, property and telemetry fields can have these [complex types](#complex-object-type-example):
* `Object`
* `Map`
* `Enum`
* (**telemetry** only) `Array`

They can also be [semantic types](#semantic-type-example), which allow you to annotate values with units.

### Basic property and telemetry examples

Here is a basic example of a **property** on a DTDL model. This example shows the ID property of a Home.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/basic-home-example/IHome.json" highlight="7-11":::

Here is a basic example of a **telemetry** field on a DTDL model. This example shows Temperature telemetry on a Sensor.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/basic-home-example/ISensor.json" highlight="7-11":::

### Complex (object) type example

Properties and telemetry can be of complex types, including an `Object` type.

The following example shows another version of the Home model, with a property for its address. `address` is an object, with its own fields for street, city, state, and zip.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/advanced-home-example/IHome.json" highlight="8-31":::

### Semantic type example

Semantic types make it possible to express a value with a unit. Properties and telemetry can be represented with any of the semantic types that are supported by DTDL. For more information on semantic types in DTDL and what values are supported, see [Semantic types in the DTDL v2 spec](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#semantic-types).

The following example shows a Sensor model with a semantic-type telemetry for Temperature, and a semantic-type property for Humidity.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/advanced-home-example/ISensor.json" highlight="7-18":::

## Relationships

This section goes into more detail about **relationships** in DTDL models.

### Basic relationship example

Here is a basic example of a relationship on a DTDL model. This example shows a relationship on a Home model that allows it to connect to a Floor model.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/basic-home-example/IHome.json" highlight="12-18":::

### Targeted and non-targeted relationships

Relationships can be defined with or without a **target**. A target specifies which types of twin the relationship can reach. For example, you might include a target to specify that a Home model can only have a *rel_has_floors* relationship with twins that are Floor twins. 

Sometimes, you might want to define a relationship without a specific target, so that the relationship can connect to many different types of twins.

Here is an example of a relationship on a DTDL model that does not have a target. In this example, the relationship is for defining what sensors a Room might have, and the relationship can connect to any type.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/advanced-home-example/IRoom.json" range="2-27" highlight="20-25":::

### Properties of relationships

DTDL also allows for **relationships** to have properties of their own. When defining a relationship within a DTDL model, the relationship can have its own `properties` field where you can define custom properties to describe relationship-specific state.

The following example shows another version of the Home model, where the `rel_has_floors` relationship has a property representing when the related Floor was last occupied.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/advanced-home-example/IHome.json" highlight="39-45":::

## Components

This section goes into more detail about **components** in DTDL models.

### Basic component example

Here is a basic example of a component on a DTDL model. This example shows a Room model that makes use of a thermostat component.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/advanced-home-example/IRoom.json" highlight="15-19, 28-41":::

> [!NOTE]
> Note that the component interface (thermostat component) is defined in the same array as the interface that uses it (Room). Components must be defined this way in API calls in order for the interface to be found.

## Model inheritance

Sometimes, you may want to specialize a model further. For example, it might be useful to have a generic model Room, and specialized variants ConferenceRoom and Gym. To express specialization, **DTDL supports inheritance**. Interfaces can inherit from one or more other interfaces. This is done by adding an `extends` field to the model.

The `extends` section is an interface name, or an array of interface names (allowing the extending interface to inherit from multiple parent models if desired). A single parent can serve as the base model for multiple extending interfaces.

The following example re-imagines the Home model from the earlier DTDL example as a subtype of a larger "core" model. The parent model (Core) is defined first, and then the child model (Home) builds on it by using `extends`.

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/advanced-home-example/ICore.json":::

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/advanced-home-example/IHome.json" range="1-8" highlight="6":::

In this case, Core contributes an ID and name to Home. Other models can also extend the Core model to get these properties as well. Here is a Room model extending the same parent interface:

:::code language="json" source="~/digital-twins-docs-samples-getting-started/models/advanced-home-example/IRoom.json" range="2-9" highlight="6":::

Once inheritance is applied, the extending interface exposes all properties from the entire inheritance chain.

The extending interface cannot change any of the definitions of the parent interfaces; it can only add to them. It also cannot redefine a capability already defined in any of its parent interfaces (even if the capabilities are defined to be the same). For example, if a parent interface defines a `double` property *mass*, the extending interface cannot contain a declaration of *mass*, even if it's also a `double`.

## Modeling best practices

While designing models to reflect the entities in your environment, it can be useful to look ahead and consider the [query](concepts-query-language.md) implications of your design. You may want to design properties in a way that will avoid large result sets from graph traversal. You may also want to model relationships that will need to be answered in a single query as single-level relationships.

### Validating models

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

## Modeling tools

There are several samples available to make it even easier to deal with models and ontologies. They are located in this repository: [Tools for Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-tools).

This section describes the current set of samples in more detail.

### Model uploader 

Once you are finished creating, extending, or selecting your models, you can upload them to your Azure Digital Twins instance to make them available for use in your solution. This is done using the [Azure Digital Twins APIs](concepts-apis-sdks.md), as described in [How-to: Manage DTDL models](how-to-manage-model.md#upload-models).

However, if you have many models to upload—or if they have many interdependencies that would make ordering individual uploads complicated—you can use the [Azure Digital Twins Model Uploader sample](https://github.com/Azure/opendigitaltwins-tools/tree/master/ADTTools#uploadmodels) to upload many models at once. Follow the instructions provided with the sample to configure and use this project to upload models into your own instance.

### Model visualizer 

Once you have uploaded models into your Azure Digital Twins instance, you can view the models in your Azure Digital Twins instance, including any inheritance and model relationships, using the [Azure Digital Twins Model Visualizer](https://github.com/Azure/opendigitaltwins-building-tools/tree/master/AdtModelVisualizer). This sample is currently in a draft state. We encourage the digital twins development community to extend and contribute to the sample. 

## Next steps

* Learn about creating models based on industry-standard ontologies: [Concepts: What is an ontology?](concepts-ontologies.md)

* Dive deeper into managing models with API operations: [How-to: Manage DTDL models](how-to-manage-model.md)

* Learn about how models are used to create digital twins: [Concepts: Digital twins and the twin graph](concepts-twins-graph.md)


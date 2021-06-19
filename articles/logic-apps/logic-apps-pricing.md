---
title: Billing & pricing models
description: Overview about how pricing and billing models work in Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: conceptual
ms.date: 06/10/2021
---

# Pricing and billing models for Azure Logic Apps

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) helps you create and run automated integration workflows that can scale in the cloud. This article describes how billing and pricing models work for Azure Logic Apps and related resources. For specific pricing rates, see [Logic Apps Pricing](https://azure.microsoft.com/pricing/details/logic-apps). To learn how you can plan, manage, and monitor costs, see [Plan and manage costs for Azure Logic Apps](plan-manage-costs.md).

<a name="consumption-pricing"></a>

## Consumption pricing (multi-tenant)

A pay-for-use consumption pricing model applies to logic apps that run in the public, "global", multi-tenant Azure Logic Apps environment. You can create these logic apps by using multiple options, for example:

* Logic App (Consumption) resource type in the Azure portal
* Azure Logic Apps (Consumption) extension in Visual Studio Code
* Azure Logic Apps Tools extension in Visual Studio
* Azure Resource Manager template (ARM template) using the `Microsoft.Logic` resource type
* Azure CLI for Azure Logic Apps using the `az logic` commands
* Azure PowerShell for Azure Logic Apps using the `Az.LogicApp` module
* REST API for Azure Logic Apps
* [Automation tasks](create-automation-tasks-azure-resources.md) in the Azure portal

Metering and billing are based on the trigger and action executions in a logic app workflow. These executions are metered and billed, regardless whether the workflow runs successfully or whether the workflow is even instantiated. For example, suppose your automation task uses a polling trigger that regularly makes an outgoing call to an endpoint. This outbound request is metered and billed as an execution, regardless whether the trigger fires or is skipped, which affects whether a workflow instance is created.

| Items | Description |
|-------|-------------|
| [Built-in](../connectors/built-in.md) triggers and actions | Run natively in the Logic Apps service and are metered using the [**Actions** price](https://azure.microsoft.com/pricing/details/logic-apps/). <p><p>For example, the HTTP trigger and Request trigger are built-in triggers, while the HTTP action and Response action are built-in actions. Data operations, batch operations, variable operations, and [workflow control actions](../connectors/built-in.md), such as loops, conditions, switch, parallel branches, and so on, are also built-in actions. <p><p>**Note**: As a monthly bonus, the Consumption plan includes several thousand built-in executions free of charge. |
| [Standard connector](../connectors/managed.md) triggers and actions <p><p>[Custom connector](../connectors/apis-list.md#custom-apis-and-connectors) triggers and actions | Metered using the [Standard connector price](https://azure.microsoft.com/pricing/details/logic-apps/). |
| [Enterprise connector](../connectors/managed.md) triggers and actions | Metered using the [Enterprise connector price](https://azure.microsoft.com/pricing/details/logic-apps/). However, during connector preview, Enterprise connectors are metered using the [*Standard* connector price](https://azure.microsoft.com/pricing/details/logic-apps/). |
| Actions inside [loops](logic-apps-control-flow-loops.md) | Each action that runs in a loop is metered for each loop cycle that runs. <p><p>For example, suppose that you have a "for each" loop that includes actions that process a list. The Logic Apps service meters each action that runs in that loop by multiplying the number of list items with the number of actions in the loop, and adds the action that starts the loop. So, the calculation for a 10-item list is (10 * 1) + 1, which results in 11 action executions. |
| Retry attempts | To handle the most basic exceptions and errors, you can set up a [retry policy](logic-apps-exception-handling.md#retry-policies) on triggers and actions where supported. These retries along with the original request are charged at rates based on whether the trigger or action has built-in, Standard, or Enterprise type. For example, an action that executes with 2 retries is charged for 3 action executions. |
| [Data retention and storage usage](#data-retention) | Metered using the data retention price, which you can find on the [Logic Apps pricing page](https://azure.microsoft.com/pricing/details/logic-apps/), under the **Pricing details** table. |
|||

For more information, review the following documentation:

* [View metrics for executions and storage usage](plan-manage-costs.md#monitor-billing-metrics)
* [Limits in Azure Logic Apps](logic-apps-limits-and-config.md)

### Not metered

* Triggers that are skipped due to unmet conditions
* Actions that didn't run because the logic app stopped before finishing
* [Disabled logic apps](#disabled-apps)

### Other related resources

Logic apps work with other related resources, such as integration accounts, on-premises data gateways, and integration service environments (ISEs). To learn about pricing for those resources, review these sections later in this topic:

* [On-premises data gateway](#data-gateway)
* [Integration account pricing model](#integration-accounts)
* [ISE pricing model](#fixed-pricing)

### Tips for estimating consumption costs

To help you estimate more accurate consumption costs, review these tips:

* Consider the possible number of messages or events that might arrive on any given day, rather than base your calculations on only the polling interval.

* When an event or message meets the trigger criteria, many triggers immediately try to read any other waiting events or messages that meet the criteria. This behavior means that even when you select a longer polling interval, the trigger fires based on the number of waiting events or messages that qualify for starting workflows. Triggers that follow this behavior include Azure Service Bus and Azure Event Hub.

  For example, suppose you set up trigger that checks an endpoint every day. When the trigger checks the endpoint and finds 15 events that meet the criteria, the trigger fires and runs the corresponding workflow 15 times. The Logic Apps service meters all the actions that those 15 workflows perform, including the trigger requests.

<a name="standard-pricing"></a>

## Standard pricing (single-tenant)

A hosting plan and pricing tier-based pricing model applies to logic apps that run in the single-tenant Azure Logic Apps environment. This pricing applies to the **Logic App (Standard)** resource type in the Azure portal or to logic apps that you work on using the **Azure Logic Apps (Standard)** extension for Visual Studio Code. When you create or deploy such a logic app, you must choose a hosting plan and pricing tier that determines the pricing rates to use for metering and billing when running your workflows.

> [!NOTE]
> For new logic apps that you create with the **Logic App (Standard)** resource type, you must use the **Workflow Standard** 
> hosting plan. The App Service Plan and App Service Environment aren't available for new logic apps.

<a name="hosting-plans"></a>

### Pricing tiers and billing rates

Each pricing tier in a hosting plan includes a specific amount of compute, memory, and storage resources. For hourly rates per resource and per region, review the [Azure Logic Apps pricing page](https://azure.microsoft.com/pricing/details/logic-apps/).

To better understand how pricing works, this example provides sample estimates for the *East US 2 region*.

* On the [Azure Logic Apps pricing page](https://azure.microsoft.com/pricing/details/logic-apps/), select the **East US 2** region to view the hourly rates, or review the following table:

  | Resource | Hourly US$ (East US 2) |
  |----------|------------------------|
  | **Virtual CPU (vCPU)** | $0.192 |
  | **Memory** | $0.0137 per GB |
  |||

* Based on the preceding information, this table shows the estimated monthly rate for each pricing tier and the resources included in that pricing tier:

  | Pricing tier | Monthly US$ (East US 2) | Virtual CPU (vCPU) | Memory (GB) | Storage (GB) |
  |--------------|-------------------------|--------------------|-------------|--------------|
  | **WS1** | $175.20 | 1 | 3.5 | 250 |
  | **WS2** | $350.40 | 2 | 7 | 250 |
  | **WS3** | $700.80 | 4 | 14 | 250 |
  ||||||

* Based on the preceding information, this table lists each resource and the estimated monthly rate if you choose the **WS1** pricing tier:

  | Resource | Amount | Monthly US$ (East US 2) |
  |----------|--------|-------------------------|
  | **Virtual CPU (vCPU)** | 1 | $140.16 |
  | **Memory** | 3.5 GB | $35.04 |
  ||||

<a name="storage-transactions"></a>

### Storage transactions

Azure Logic Apps uses [Azure Storage](../storage/index.yml) for any storage operations. With multi-tenant Azure Logic Apps, any storage usage and costs are attached to the logic app. With single-tenant Azure Logic Apps, you can use your own Azure [storage account](../azure-functions/storage-considerations.md#storage-account-requirements). This capability gives you more control and flexibility with your Logic Apps data.

When *stateful* workflows run their operations, the Azure Logic Apps runtime makes storage transactions. For example, queues are used for scheduling, while tables and blobs are used for storing workflow states. Storage costs change based on your workflow's content. Different triggers, actions, and payloads result in different storage operations and needs. Storage transactions follow the [Azure Storage pricing model](https://azure.microsoft.com/pricing/details/storage/). Storage costs are listed separately in your Azure billing invoice.

### Tips for estimating storage needs and costs

To help you get some idea about the number of storage operations that a workflow might run and their cost, try using the [Logic Apps Storage calculator](https://logicapps.azure.com/calculator). You can either select a sample workflow or use an existing workflow definition. The first calculation estimates the number of storage operations in your workflow. You can then use these numbers to estimate possible costs using the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/).

For more information, review the following documentation:

* [Estimate storage needs and costs for workflows in single-tenant Azure Logic Apps](estimate-storage-costs.md)
* [Azure Storage pricing details](https://azure.microsoft.com/pricing/details/storage/)

<a name="fixed-pricing"></a>

## ISE pricing (dedicated)

A fixed pricing model applies to logic apps that run in the dedicated [*integration service environment* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). An ISE is billed using the [Integration Service Environment price](https://azure.microsoft.com/pricing/details/logic-apps), which depends on the [ISE level or *SKU*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) that you create. This pricing differs from multi-tenant pricing as you're paying for reserved capacity and dedicated resources whether or not you use them.

| ISE SKU | Description |
|---------|-------------|
| **Premium** | The base unit has [fixed capacity](logic-apps-limits-and-config.md#integration-service-environment-ise) and is [billed at an hourly rate for the Premium SKU](https://azure.microsoft.com/pricing/details/logic-apps). If you need more throughput, you can [add more scale units](../logic-apps/ise-manage-integration-service-environment.md#add-capacity) when you create your ISE or afterwards. Each scale unit is billed at an [hourly rate that's roughly half the base unit rate](https://azure.microsoft.com/pricing/details/logic-apps). <p><p>For capacity and limits information, see [ISE limits in Azure Logic Apps](logic-apps-limits-and-config.md#integration-service-environment-ise). |
| **Developer** | The base unit has [fixed capacity](logic-apps-limits-and-config.md#integration-service-environment-ise) and is [billed at an hourly rate for the Developer SKU](https://azure.microsoft.com/pricing/details/logic-apps). However, this SKU has no service-level agreement (SLA), scale up capability, or redundancy during recycling, which means that you might experience delays or downtime. Backend updates might intermittently interrupt service. <p><p>**Important**: Make sure that you use this SKU only for exploration, experiments, development, and testing - not for production or performance testing. <p><p>For capacity and limits information, see [ISE limits in Azure Logic Apps](logic-apps-limits-and-config.md#integration-service-environment-ise). |
|||

### Included at no extra cost

| Items | Description |
|-------|-------------|
| [Built-in](../connectors/built-in.md) triggers and actions | Display the **Core** label and run in the same ISE as your logic apps. |
| [Standard connectors](../connectors/managed.md) <p><p>[Enterprise connectors](../connectors/managed.md#enterprise-connectors) | - Managed connectors that display the **ISE** label are specially designed to work without the on-premises data gateway and run in the same ISE as your logic apps. ISE pricing includes as many Enterprise connections as you want. <p><p>- Connectors that don't display the ISE label run in the single-tenant Azure Logic Apps service. However, ISE pricing includes these executions for logic apps that run in an ISE. |
| Actions inside [loops](logic-apps-control-flow-loops.md) | ISE pricing includes each action that runs in a loop for each loop cycle that runs. <p><p>For example, suppose that you have a "for each" loop that includes actions that process a list. To get the total number of action executions, multiply the number of list items with the number of actions in the loop, and add the action that starts the loop. So, the calculation for a 10-item list is (10 * 1) + 1, which results in 11 action executions. |
| Retry attempts | To handle the most basic exceptions and errors, you can set up a [retry policy](logic-apps-exception-handling.md#retry-policies) on triggers and actions where supported. ISE pricing includes retries along with the original request. |
| [Data retention and storage usage](#data-retention) | Logic apps in an ISE don't incur charges for data retention and storage usage. |
| [Integration accounts](#integration-accounts) | Includes usage for a single integration account tier, based on ISE SKU, at no extra cost. |
|||

For limits information, see [ISE limits in Azure Logic Apps](logic-apps-limits-and-config.md#integration-service-environment-ise).

<a name="integration-accounts"></a>

## Integration accounts

An [integration account](../logic-apps/logic-apps-pricing.md#integration-accounts) is a separate resource, which you link create link to a logic app so that you can explore, build, and test B2B integration solutions that use [EDI](logic-apps-enterprise-integration-b2b.md) and [XML processing](logic-apps-enterprise-integration-xml.md) capabilities.

Azure Logic Apps offers these integration account levels or tiers that [vary in pricing](https://azure.microsoft.com/pricing/details/logic-apps/) and [billing model](logic-apps-pricing.md#integration-accounts), based on whether your logic app runs in Azure Logic Apps (multi-tenant or single-tenant) or an ISE.

| Tier | Description |
|------|-------------|
| **Basic** | For scenarios where you want only message handling or to act as a small business partner that has a trading partner relationship with a larger business entity. <p><p>Supported by the Logic Apps SLA. |
| **Standard** | For scenarios where you have more complex B2B relationships and increased numbers of entities that you must manage. <p><p>Supported by the Logic Apps SLA. |
| **Free** | For exploratory scenarios, not production scenarios. This tier has limits on region availability, throughput, and usage. For example, the Free tier is available only for public regions in Azure, for example, West US or Southeast Asia, but not for [Azure China 21Vianet](/azure/china/overview-operations) or [Azure Government](../azure-government/documentation-government-welcome.md). <p><p>**Note**: Not supported by the Logic Apps SLA. |
|||

For information about integration account limits, see [Limits and configuration for Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits), such as:

* [Limits on integration accounts per Azure subscription](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits)

* [Limits on various artifacts per integration account](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits). Artifacts include trading partners, agreements, maps, schemas, assemblies, certificates, batch configurations, and so on.

### Multi-tenant or single-tenant based logic apps

Integration accounts are billed using a fixed [integration account price](https://azure.microsoft.com/pricing/details/logic-apps/) that is based on the account tier that you use.

### ISE-based logic apps

At no extra cost, your ISE includes a single integration account, based on your ISE SKU. For an extra cost, you can create more integration accounts for your ISE to use up to the [total ISE limit](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits). Learn more about the [ISE pricing model](#fixed-pricing) earlier in this topic.

| ISE SKU | Included integration account | Extra cost |
|---------|------------------------------|-----------------|
| **Premium** | A single [Standard](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) integration account | Up to 19 more Standard accounts. No Free or Basic accounts are permitted. |
| **Developer** | A single [Free](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) integration account | Up to 19 more Standard accounts if you already have a Free account, or 20 total Standard accounts if you don't have a Free account. No Basic accounts are permitted. |
||||

<a name="data-retention"></a>

## Data retention and storage usage

Azure Logic Apps uses [Azure Storage](../storage/index.yml) for any storage operations. The inputs and outputs from your workflow's run history are stored and metered, based on your logic app's [run history retention limit](logic-apps-limits-and-config.md#run-duration-retention-limits). To monitor storage usage, see [View metrics for executions and storage usage](plan-manage-costs.md#monitor-billing-metrics).

| Environment | Notes |
|-------------|-------|
| **Multi-tenant** | Storage usage and retention are billed using a fixed rate, which you can find on the [Logic Apps pricing page](https://azure.microsoft.com/pricing/details/logic-apps), under the **Pricing details** table. |
| **Single-tenant** | Storage usage and retention are billed using the [Azure Storage pricing model](https://azure.microsoft.com/pricing/details/storage/). Storage costs are listed separately in your Azure billing invoice. For more information, review [Storage transactions (single-tenant)](#storage-transactions). |
| **ISE** | Storage usage and retention don't incur charges. |
|||

<a name="data-gateway"></a>

## On-premises data gateway

An [on-premises data gateway](../logic-apps/logic-apps-gateway-install.md) is a separate resource that you create so that your logic app workflows can access on-premises data by using specific gateway-supported connectors. Connectors operations that run through the gateway incur charges, but the gateway itself doesn't incur charges.

<a name="disabled-apps"></a>

## Disabled logic apps or workflows

Disabled logic apps (multi-tenant) or workflows (single-tenant) don't incur charges because they can't create new instances while they're disabled.

## Next steps

* [Plan and manage costs for Azure Logic Apps](plan-manage-costs.md)
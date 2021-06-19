---
title: Azure Cosmos DB integrated cache frequently asked questions
description: Frequently asked questions about the Azure Cosmos DB integrated cache.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/25/2021
ms.author: tisande
---

# Azure Cosmos DB integrated cache frequently asked questions
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

The Azure Cosmos DB integrated cache is an in-memory cache that is built-in to Azure Cosmos DB. This article answers commonly asked questions about the Azure Cosmos DB integrated cache.

## Frequently asked questions

### Why does the integrated cache require a dedicated gateway?

If you’ve connected to Azure Cosmos DB using gateway mode, you’ve used the standard gateway. While the Azure Cosmos DB backend (your provisioned throughput and storage) has dedicated capacity per container, the standard gateway is shared among many customers. It is practical for many customers to share a standard gateway since the compute resources consumed by each individual customer are minimal. Because the integrated cache is specific to your Azure Cosmos DB account and requires significant CPU and memory, it requires a dedicated gateway node.

### What is a dedicated gateway?

A [dedicated gateway](dedicated-gateway.md) is server-side compute that is a front-end to data in an Azure Cosmos DB account. When you connect to your dedicated gateway endpoint, your application sends a request to the dedicated gateway, which then routes the request to different backend partitions.

### Does using the dedicated gateway offer any other performance benefits over using the standard gateway?

In general, requests routed by the dedicated gateway will have a slightly lower and more consistent latency than requests routed by the standard gateway. Even requests that don't use the integrated cache will still have a slightly lower latency than the standard gateway.

### What kind of latency should I expect from the integrated cache?

A request served by the integrated cache is faster because the cached data is stored in-memory on the dedicated gateway, rather than on the backend. For cached point reads, you should expect latency of 2-4 ms.

For cached queries, latency depends on the query. The query cache works by caching the query engine’s response for a particular query. This response is then sent back client-side to the SDK for processing. For simple queries, minimal work in the SDK is required and latencies of 2-4 ms are typical. However, more complex queries with `GROUP BY` or `DISTINCT` require more processing in the SDK so latency may be higher, even with the query cache.

If you were previously connecting to Azure Cosmos DB with direct mode and switch to connecting with the dedicated gateway, you may observe a slight latency increase for some requests. Using gateway mode requires a request to be sent to the gateway (in this case the dedicated gateway) and then routed appropriately to the backend. Direct mode, as the name suggests, allows the client to communicate directly with the backend, removing an extra hop. 

If your app previously used direct mode, the latency advantages of the integrated cache will be significant in only the following scenarios:

- Point read latency for large items (> 16 KB)
- High RU or complex queries

If your app previously used gateway mode with the standard gateway, the integrated cache will offer reductions in latency in nearly all scenarios. 

### Does the Azure Cosmos DB availability SLA extend to the dedicated gateway and integrated cache?

We will have an availability SLA/SLO on the dedicated gateway (and therefore the integrated cache) once the feature is generally available. For scenarios that require high availability, you should provision 3x the number of dedicated gateway instances needed. For example, if one dedicated gateway node is needed in production, you should provision two additional dedicated gateway nodes to account for possible downtime or outages.

### The integrated cache is only available for SQL (Core) API right now. Are you planning on releasing it for other APIs as well?

Expanding the integrated cache beyond SQL API is planned on the long-term roadmap but beyond the initial public preview of the integrated cache.

## Next steps

- [Integrated cache](integrated-cache.md)
- [Configure the integrated cache](how-to-configure-integrated-cache.md)
- [Dedicated gateway](dedicated-gateway.md)
---
title: Monitor Azure Cosmos DB data by using Azure Diagnostic settings
description: Learn how to use Azure diagnostic settings to monitor the performance and availability of data stored in Azure Cosmos DB
author: SnehaGunda
services: cosmos-db
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/20/2021
ms.author: sngun
---

# Monitor Azure Cosmos DB data by using diagnostic settings in Azure
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Diagnostic settings in Azure are used to collect resource logs. Azure resource Logs are emitted by a resource and provide rich, frequent data about the operation of that resource. These logs are captured per request and they are also referred to as "data plane logs". Some examples of the data plane operations include delete, insert, and readFeed. The content of these logs varies by resource type.

Platform metrics and the Activity logs are collected automatically, whereas you must create a diagnostic setting to collect resource logs or forward them outside of Azure Monitor. You can turn on diagnostic setting for Azure Cosmos DB accounts and send resource logs to the following sources:
- Log Analytics workspaces
  - Data sent to Log Analytics can be written into **Azure Diagnostics (legacy)** or **Resource-specific (preview)** tables 
- Event hub
- Storage Account
  
> [!NOTE]
> For SQL API accounts, we recommend creating the diagnostic setting in resource specific mode [following our instructions for creating diagnostics setting via REST API](cosmosdb-monitor-resource-logs.md#create-diagnostic-setting). This option provides additional cost-optimizations with an improved view for handling data.

## Create using the Azure portal

1. Sign into the [Azure portal](https://portal.azure.com).

1. Navigate to your Azure Cosmos account. Open the **Diagnostic settings** pane, and then select **Add diagnostic setting** option.

2. In the **Diagnostic settings** pane, fill the form with your preferred categories.

### Choosing Log Categories

|Category  |API   | Definition  | Key Properties   |
|---------|---------|---------|---------|
|DataPlaneRequests     |  All APIs        |     Logs back-end requests as data plane operations which are requests executed to create, update, delete or retrieve data within the account.   |   `Requestcharge`, `statusCode`, `clientIPaddress`, `partitionID`, `resourceTokenPermissionId` `resourceTokenPermissionMode`      |
|MongoRequests     |    Mongo    |   Logs user-initiated requests from the front end to serve requests to Azure Cosmos DB's API for MongoDB. When you enable this category, make sure to disable DataPlaneRequests.      |  `Requestcharge`, `opCode`, `retryCount`, `piiCommandText`      |
|CassandraRequests     |   Cassandra      |    Logs user-initiated requests from the front end to serve requests to Azure Cosmos DB's API for Cassandra. When you enable this category, make sure to disable DataPlaneRequests.     |     `operationName`, `requestCharge`, `piiCommandText`    |
|GremlinRequests     |    Gremlin    |     Logs user-initiated requests from the front end to serve requests to Azure Cosmos DB's API for Gremlin. When you enable this category, make sure to disable DataPlaneRequests.    |   `operationName`, `requestCharge`, `piiCommandText`, `retriedDueToRateLimiting`       |
|QueryRuntimeStatistics     |   SQL      |     This table details query operations executed against a SQL API account. By default, the query text and its parameters are obfuscated to avoid logging PII data with full text query logging available by request.    |    `databasename`, `partitionkeyrangeid`, `querytext`    |
|PartitionKeyStatistics     |    All APIs     |   Logs the statistics of logical partition keys by representing the storage size (KB) of the partition keys. This table is useful when troubleshooting storage skews.      |   `subscriptionId`, `regionName`, `partitionKey`, `sizeKB`      |
|PartitionKeyRUConsumption     |   SQL API    |     Logs the aggregated per-second RU/s consumption of partition keys. This table is useful for troubleshooting hot partitions. Currently, Azure Cosmos DB reports partition keys for SQL API accounts only and for point read/write and stored procedure operations.   |     `subscriptionId`, `regionName`, `partitionKey`, `requestCharge`, `partitionKeyRangeId`   |
|ControlPlaneRequests     |   All APIs       |    Logs details on control plane operations i.e. creating an account, adding or removing a region, updating account replication settings etc.     |    `operationName`, `httpstatusCode`, `httpMethod`, `region`       |
|TableApiRequests     |   Table API    |     Logs user-initiated requests from the front end to serve requests to Azure Cosmos DB's API for Table. When you enable this category, make sure to disable DataPlaneRequests.       |    `operationName`, `requestCharge`, `piiCommandText`     |


## <a id="create-diagnostic-setting"></a> Create diagnostic setting via REST API
Use the [Azure Monitor REST API](/rest/api/monitor/diagnosticsettings/createorupdate)
 for creating a diagnostic setting via the interactive console.
> [!Note]
> If you are using SQL API, we recommend setting the **logAnalyticsDestinationType** property to **Dedicated** for enabling resource specific tables.

### Request

```HTTP
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

### Headers

|Parameters/Headers  | Value/Description  |
|---------|---------|
|name     |  The name of your Diagnostic setting.      |
|resourceUri     |   subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.DocumentDb/databaseAccounts/{ACCOUNT_NAME}/providers/microsoft.insights/diagnosticSettings/{DIAGNOSTIC_SETTING_NAME}      |
|api-version     |    2017-05-01-preview     |
|Content-Type     |    application/json     |

### Body

```json
{
    "id": "/subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.DocumentDb/databaseAccounts/{ACCOUNT_NAME}/providers/microsoft.insights/diagnosticSettings/{DIAGNOSTIC_SETTING_NAME}",
    "type": "Microsoft.Insights/diagnosticSettings",
    "name": "name",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": null,
        "serviceBusRuleId": null,
        "workspaceId": "/subscriptions/{SUBSCRIPTION_ID}/resourcegroups/{RESOURCE_GROUP}/providers/microsoft.operationalinsights/workspaces/{WORKSPACE_NAME}",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "logs": [
            {
                "category": "DataPlaneRequests",
                "categoryGroup": null,
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "QueryRuntimeStatistics",
                "categoryGroup": null,
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "PartitionKeyStatistics",
                "categoryGroup": null,
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "PartitionKeyRUConsumption",
                "categoryGroup": null,
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ControlPlaneRequests",
                "categoryGroup": null,
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ],
        "logAnalyticsDestinationType": "Dedicated"
    },
    "identity": null
}
```

## Create diagnostic setting via Azure CLI
Use the [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create) command to create a diagnostic setting with the Azure CLI. See the documentation for this command for descriptions of its parameters.

> [!Note]
> If you are using SQL API, we recommend setting the **export-to-resource-specific** property to **true**.

```azurecli-interactive
az monitor diagnostic-settings create --resource /subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.DocumentDb/databaseAccounts/ --name {DIAGNOSTIC_SETTING_NAME} --export-to-resource-specific true --logs '[{"category": "QueryRuntimeStatistics","categoryGroup": null,"enabled": true,"retentionPolicy": {"enabled": false,"days": 0}}]' --workspace /subscriptions/{SUBSCRIPTION_ID}/resourcegroups/{RESOURCE_GROUP}/providers/microsoft.operationalinsights/workspaces/{WORKSPACE_NAME}"
```

## Next steps
* For more information on how to query resource-specific tables see [troubleshooting using resource specific tables](cosmosdb-monitor-logs-basic-queries.md#resource-specific-queries).

* For more information on how to query AzureDiagnostics tables see [troubleshooting using AzureDiagnostics tables](cosmosdb-monitor-logs-basic-queries.md#azure-diagnostics-queries).

* For detailed information about how to create a diagnostic setting by using the Azure portal, CLI, or PowerShell, see [create diagnostic setting to collect platform logs and metrics in Azure](../azure-monitor/essentials/diagnostic-settings.md) article.
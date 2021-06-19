---
title: Edit runtime and environment settings in single-tenant Azure Logic Apps
description: Change the runtime and environment settings for logic apps in single-tenant Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: how-to
ms.date: 05/25/2021
---

# Edit host and app settings for logic apps in single-tenant Azure Logic Apps

In *single-tenant* Azure Logic Apps, the *app settings* for a logic app specify the global configuration options that affect *all the workflows* in that logic app. However, these settings apply *only* when these workflows run in your *local development environment*. While running locally, the workflows can access these app settings as *local environment variables*, which are used by local development tools for values that can often change between environments. For example, these values can contain connection strings. When you deploy to Azure, app settings are ignored and aren't included with your deployment.

Your logic app also has *host settings*, which specify the runtime configuration settings and values that apply to *all the workflows* in that logic app, for example, default values for throughput, capacity, data size, and so on, *whether they run locally or in Azure*.

<a name="app-settings-parameters-deployment"></a>

## App settings, parameters, and deployment

In *multi-tenant* Azure Logic Apps, deployment depends on Azure Resource Manager templates (ARM templates), which combine and handle resource provisioning for both logic apps and infrastructure. This design poses a challenge when you have to maintain environment variables for logic apps across across various dev, test, and production environments. Everything in an ARM template is defined at deployment. If you need to change just a single variable, you have to redeploy everything.

In *single-tenant* Azure Logic Apps, deployment becomes easier because you can separate resource provisioning between apps and infrastructure. You can use *parameters* to abstract values that might change between environments. By defining parameters to use in your workflows, you can first focus on designing your workflows, and then insert your environment-specific variables later. You can call and reference your environment variables at runtime by using app settings and parameters. That way, you don't have to redeploy as often.

App settings integrate with Azure Key Vault. You can [directly reference secure strings](../app-service/app-service-key-vault-references.md), such as connection strings and keys. Similar to Azure Resource Manager templates (ARM templates), where you can define environment variables at deployment time, you can define app settings within your [logic app workflow definition](/azure/templates/microsoft.logic/workflows). You can then capture dynamically generated infrastructure values, such as connection endpoints, storage strings, and more. However, app settings have size limitations and can't be referenced from certain areas in Azure Logic Apps.

For more information about setting up your logic apps for deployment, see the following documentation:

- [Create parameters for values that change in workflows between environments for single-tenant Azure Logic Apps](parameterize-workflow-app.md)
- [DevOps deployment overview for single-tenant based logic apps](devops-deployment-single-tenant-azure-logic-apps.md)
- [Set up DevOps deployment for single-tenant based logic apps](set-up-devops-deployment-single-tenant-azure-logic-apps.md)

## Visual Studio Code project structure

[!INCLUDE [Visual Studio Code - logic app project structure](../../includes/logic-apps-single-tenant-project-structure-visual-studio-code.md)]

<a name="reference-local-settings-json"></a>

## Reference for app settings - local.settings.json

In Visual Studio Code, at your logic app project's root level, the **local.settings.json** file contain global configuration options that affect *all workflows* in that logic app while running in your local development environment. When your workflows run locally, these settings are accessed as local environment variables, and their values can often change between the various environments where you run your workflows. To view and manage these settings, review [Manage app settings - local.settings.json](#manage-app-settings).

App settings in Azure Logic Apps work similarly to app settings in Azure Functions or Azure Web Apps. If you've used these other services before, you might already be familiar with app settings. For more information, review [App settings reference for Azure Functions](../azure-functions/functions-app-settings.md) and [Work with Azure Functions Core Tools - Local settings file](../azure-functions/functions-run-local.md#local-settings-file).

| Setting | Default value | Description |
|---------|---------------|-------------|
| `AzureWebJobsStorage` | None | Sets the connection string for an Azure storage account. |
| `Workflows.<workflowName>.FlowState` | None | Sets the state for <*workflowName*>. |
| `Workflows.<workflowName>.RuntimeConfiguration.RetentionInDays` | None | Sets the operation options for <*workflowName*>. |
| `Workflows.Connection.AuthenticationAudience` | None | Sets the audience for authenticating an Azure-hosted connection. |
| `Workflows.WebhookRedirectHostUri` | None | Sets the host name to use for webhook callback URLs. |
| `WEBSITE_LOAD_ROOT_CERTIFICATES` | None | Sets the thumbprints for the root certificates to be trusted. |
||||

<a name="manage-app-settings"></a>

## Manage app settings - local.settings.json

To add, update, or delete app settings, select and review the following sections for Visual Studio Code, Azure portal, Azure CLI, or ARM (Bicep) template. For app settings specific to logic apps, review the [reference guide for available app settings - local.settings.json](#reference-local-settings-json).

### [Visual Studio Code](#tab/visual-studio-code)

To review the app settings for your logic app in Visual Studio Code, follow these steps:

1. In your logic app project, at the root project level, find and open the **local.settings.json** file.

1. In the `Values` object, review the app settings for your logic app.

   For more information about these settings, review the [reference guide for available app settings - local.settings.json](#reference-local-settings-json).

To add an app setting, follow these steps:

1. In the **local.settings.json** file, find the `Values` object.

1. In the `Values` object, add the app setting that you want to apply when running locally in Visual Studio Code. Some settings enable you to add a setting for a specific workflow, for example:

   ```json
   {
      "IsEncrypted": false,
      "Values": {
         "AzureWebJobsStorage": "UseDevelopmentStorage=true",
         "Workflows.WorkflowName1.FlowState" : "Disabled",
         <...>
     }
   }
   ```

### [Azure portal](#tab/azure-portal)

To review the app settings for your single-tenant based logic app in the Azure portal, follow these steps:

1. In the [Azure portal](https://portal.azure.com/) search box, find and open your logic app.

1. On your logic app menu, under **Settings**, select **Configuration**.

1. On the **Configuration** page, on the **Application settings** tab, review the app settings for your logic app.

   For more information about these settings, review the [reference guide for available app settings - local.settings.json](#reference-local-settings-json).

1. To view all values, select **Show Values**. Or, to view a single value, select that value.

To add a setting, follow these steps:

1. On the **Application settings** tab, under **Application settings**, select **New application setting**.

1. For **Name**, enter the *key* or name for your new setting.

1. For **Value**, enter the value for your new setting.

1. When you're ready to create your new *key-value* pair, select **OK**.

:::image type="content" source="./media/edit-app-settings-host-settings/portal-app-settings-values.png" alt-text="Screenshot showing the Azure portal and the configuration pane with the app settings and values for a single-tenant based logic app." lightbox="./media/edit-app-settings-host-settings/portal-app-settings-values.png":::

### [Azure CLI](#tab/azure-cli)

To review your current app settings using the Azure CLI, run the command, `az logicapp config appsettings list`. Make sure that your command includes the `--name -n` and `--resource-group -g` parameters, for example:

```azurecli
az logicapp config appsettings list --name MyLogicApp --resource-group MyResourceGroup
```

For more information about these settings, review the [reference guide for available app settings - local.settings.json](#reference-local-settings-json).

To add or update an app setting using the Azure CLI, run the command `az logicapp config appsettings set`. Make sure that your command includes the `--name n` and `--resource-group -g` parameters. For example, the following command creates a setting with a key named `CUSTOM_LOGIC_APP_SETTING` with a value of `12345`:

```azurecli
az logicapp config appsettings set --name MyLogicApp --resource-group MyResourceGroup --settings CUSTOM_LOGIC_APP_SETTING=12345 
```

---

<a name="reference-host-json"></a>

## Reference for host settings - host.json

In Visual Studio Code, at your logic app project's root level, the **host.json** metadata file contains the runtime settings and default values that apply to *all workflows* in a logic app resource whether running locally or in Azure. To view and manage these settings, review [Manage host settings - host.json](#manage-host-settings). You can also find related limits information in the [Limits and configuration for Azure Logic Apps](logic-apps-limits-and-config.md#definition-limits) documentation.

<a name="job-orchestration"></a>

### Job orchestration throughput

These settings affect the throughput and capacity for single-tenant Azure Logic Apps to run workflow operations.

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Jobs.BackgroundJobs.DispatchingWorkersPulseInterval` | `00:00:01` <br>(1 sec) | Sets the interval for job dispatchers to poll the job queue when the previous poll returns no jobs. Job dispatchers poll the queue immediately when the previous poll returns a job. |
| `Jobs.BackgroundJobs.NumWorkersPerProcessorCount` | `192` dispatcher worker instances | Sets the number of *dispatcher worker instances* or *job dispatchers* to have per processor core. This value affects the number of workflow runs per core. |
| `Jobs.BackgroundJobs.NumPartitionsInJobTriggersQueue` | `1` job queue | Sets the number of job queues monitored by job dispatchers for jobs to process. This value also affects the number of storage partitions where job queues exist. |
| `Jobs.BackgroundJobs.NumPartitionsInJobDefinitionsTable` | `4` job partitions | Sets the number of job partitions in the job definition table. This value controls how much execution throughput is affected by partition storage limits. |
||||

<a name="run-duration-history"></a>

### Run duration and history

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.FlowRetentionThreshold` | `90.00:00:00` <br>(90 days) | Sets the amount of time to keep workflow run history after a run starts. |
| `Runtime.Backend.FlowRunTimeout` | `90.00:00:00` <br>(90 days) | Sets the amount of time a workflow can continue running before forcing a timeout. <p><p>**Important**: Make sure this value is less than or equal to the `Runtime.FlowRetentionThreshold` value. Otherwise, run histories can get deleted before the associated jobs are complete. |
||||

<a name="inputs-outputs"></a>

### Inputs and outputs

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.FlowRunActionJob.MaximumActionResultSize` | `209715200` bytes | Sets the maximum size in bytes that the combined inputs and outputs can have in an action. |
| `Runtime.ContentLink.MaximumContentSizeInBytes` | `104857600` characters | Sets the maximum size in characters that an input or output can have in a trigger or action. |
||||

<a name="pagination"></a>

### Pagination

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.FlowRunRetryableActionJobCallback.MaximumPageCount` | `1000` pages | When pagination is supported and enabled on an operation, sets the maximum number of pages to return or process at runtime. |
||||

<a name="chunking"></a>

### Chunking

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.FlowRunRetryableActionJobCallback.MaximumContentLengthInBytesForPartialContent` | `1073741824` bytes | When chunking is supported and enabled on an operation, sets the maximum size in bytes for downloaded or uploaded content. |
| `Runtime.FlowRunRetryableActionJobCallback.MaxChunkSizeInBytes` | `52428800` bytes | When chunking is supported and enabled on an operation , sets the maximum size in bytes for each content chunk. |
| `Runtime.FlowRunRetryableActionJobCallback.MaximumRequestCountForPartialContent` | `1000` requests | When chunking is supported and enabled on an operation, sets the maximum number of requests that an action execution can make to download content. |
||||

<a name="trigger-concurrency"></a>

### Trigger concurrency

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.Trigger.MaximumRunConcurrency` | `100` runs | Sets the maximum number of concurrent runs that a trigger can start. This value appears in the trigger's concurrency definition. |
| `Runtime.Trigger.MaximumWaitingRuns` | `200` runs | Sets the maximum number of runs that can wait after concurrent runs meet the maximum. This value appears in the trigger's concurrency definition. |
||||

<a name="for-each-loop"></a>

### For each loops

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.Backend.FlowDefaultForeachItemsLimit` | `100000` <br>(100K array items) | For a *stateful workflow*, sets the maximum number of array items to process in a `For each` loop. |
| `Runtime.Backend.Stateless.FlowDefaultForeachItemsLimit` | `100` items | For a *stateless workflow*, sets the maximum number of array items to process in a `For each` loop. |
| `Runtime.Backend.ForeachDefaultDegreeOfParallelism` | `20` iterations | Sets the default number of concurrent iterations, or degree of parallelism, in a `For each` loop. To run sequentially, set the value to `1`. |
| `Runtime.Backend.FlowDefaultSplitOnItemsLimit` | `100000` <br>(100K array items) | Sets the maximum number of array items to debatch or split into multiple workflow instances based on the `SplitOn` setting. |
||||

<a name="until-loop"></a>

### Until loops

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.Backend.MaximumUntilLimitCount` | `5000` iterations | For a *stateful workflow*, sets the maximum number possible for the `Count` property in an `Until` action. |
| `Runtime.Backend.Stateless.MaximumUntilLimitCount` | `100` iterations | For a *stateless workflow*, sets the maximum number possible for the `Count` property in an `Until` action. |
| `Runtime.Backend.Stateless.FlowRunTimeout` | `00:05:00` <br>(5 min) | Sets the maximum wait time for an `Until` loop in a stateless workflow. |
||||

<a name="variables"></a>

### Variables

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.Backend.DefaultAppendArrayItemsLimit` | `100000` <br>(100K array items) | Sets the maximum number of items in a variable with the Array type. |
| `Runtime.Backend.VariableOperation.MaximumVariableSize` | Stateful workflow: `104857600` characters <p><p>Stateless workflow: `1024` characters | Sets the maximum size in characters for the content that a variable can store. |
||||

<a name="http-webhook"></a>

### HTTP operations

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.Backend.HttpOperation.RequestTimeout` | `00:03:45` <br>(3 min and 45 sec) | Sets the request timeout value for HTTP triggers and actions. |
| `Runtime.Backend.HttpOperation.MaxContentSize` | `104857600` bytes | Sets the maximum request size in bytes for HTTP triggers and actions. |
| `Runtime.Backend.HttpOperation.DefaultRetryCount` | `4` retries | Sets the default retry count for HTTP triggers and actions. |
| `Runtime.Backend.HttpOperation.DefaultRetryInterval` | `00:00:07` <br>(7 sec) | Sets the default retry interval for HTTP triggers and actions. |
| `Runtime.Backend.HttpOperation.DefaultRetryMaximumInterval` | `01:00:00` <br>(1 hour) | Sets the maximum retry interval for HTTP triggers and actions. |
| `Runtime.Backend.HttpOperation.DefaultRetryMinimumInterval` | `00:00:05` <br>(5 sec) | Sets the minimum retry interval for HTTP triggers and actions. |
||||

<a name="http-webhook"></a>

### HTTP Webhook operations

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.Backend.HttpWebhookOperation.RequestTimeout` | `00:02:00` <br>(2 min) | Sets the request timeout value for HTTP webhook triggers and actions. |
| `Runtime.Backend.HttpWebhookOperation.MaxContentSize` | `104857600` bytes | Sets the maximum request size in bytes for HTTP webhook triggers and actions. |
| `Runtime.Backend.HttpWebhookOperation.DefaultRetryCount` | `4` retries | Sets the default retry count for HTTP webhook triggers and actions. |
| `Runtime.Backend.HttpWebhookOperation.DefaultRetryInterval` | `00:00:07` <br>(7 sec) | Sets the default retry interval for HTTP webhook triggers and actions. |
| `Runtime.Backend.HttpWebhookOperation.DefaultRetryMaximumInterval` | `01:00:00` <br>(1 hour) | Sets the maximum retry interval for HTTP webhook triggers and actions. |
| `Runtime.Backend.HttpWebhookOperation.DefaultRetryMinimumInterval` | `00:00:05` <br>(5 sec) | Sets the minimum retry interval for HTTP webhook triggers and actions. |
| `Runtime.Backend.HttpWebhookOperation.DefaultWakeUpInterval` | `01:00:00` <br>(1 hour) | Sets the default wakeup interval for HTTP webhook trigger and action jobs. |
||||

<a name="built-in-azure-functions"></a>

### Built-in Azure Functions operations

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.Backend.FunctionOperation.RequestTimeout` | `00:03:45` <br>(3 min and 45 sec) | Sets the request timeout value for Azure Functions actions. |
| `Runtime.Backend.FunctionOperation.MaxContentSize` | `104857600` bytes | Sets the maximum request size in bytes for Azure Functions actions. |
| `Runtime.Backend.FunctionOperation.DefaultRetryCount` | `4` retries | Sets the default retry count for Azure Functions actions. |
| `Runtime.Backend.FunctionOperation.DefaultRetryInterval` | `00:00:07` <br>(7 sec) | Sets the default retry interval for Azure Functions actions. |
| `Runtime.Backend.FunctionOperation.DefaultRetryMaximumInterval` | `01:00:00` <br>(1 hour) | Sets the maximum retry interval for Azure Functions actions. |
| `Runtime.Backend.FunctionOperation.DefaultRetryMinimumInterval` | `00:00:05` <br>(5 sec) | Sets the minimum retry interval for Azure Functions actions. |
||||

<a name="built-in-sql"></a>

### Built-in SQL operations

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.ServiceProviders.Sql.QueryExecutionTimeout` | `00:00:30` <br>(30 sec) | Sets the request timeout value for SQL service provider operations. |
||||

<a name="built-in-service-bus"></a>

### Built-in Azure Service Bus operations

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.ServiceProviders.ServiceBus.MessageSenderPoolSizePerProcessorCount` | `64` message senders | Sets the number of Azure Service Bus message senders per processor core to use in the message sender pool. |
||||

<a name="managed-api-connector"></a>

### Managed API connector operations

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.Backend.ApiConnectionOperation.RequestTimeout` | `00:02:00` <br>(2 min) | Sets the request timeout value for managed API connector triggers and actions. |
| `Runtime.Backend.ApiConnectionOperation.MaxContentSize` | `104857600` bytes | Sets the maximum request size in bytes for managed API connector triggers and actions. |
| `Runtime.Backend.ApiConnectionOperation.DefaultRetryCount` | `4` retries | Sets the default retry count for managed API connector triggers and actions. |
| `Runtime.Backend.ApiConnectionOperation.DefaultRetryInterval` | `00:00:07` <br>(7 sec) | Sets the default retry interval for managed API connector triggers and actions. |
| `Runtime.Backend.ApiWebhookOperation.DefaultRetryMaximumInterval` | `01:00:00` <br>(1 day) | Sets the maximum retry interval for managed API connector webhook triggers and actions. |
| `Runtime.Backend.ApiConnectionOperation.DefaultRetryMinimumInterval` | `00:00:05` <br>(5 sec) | Sets the minimum retry interval for managed API connector triggers and actions. |
| `Runtime.Backend.ApiWebhookOperation.DefaultWakeUpInterval` | `01:00:00` <br>(1 day) | Sets the default wakeup interval for managed API connector webhook trigger and action jobs. |
||||

<a name="blob-storage"></a>

### Blob storage

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.ContentStorage.RequestOptionsServerTimeout` | `00:00:30` <br>(30 sec) | Sets the timeout value for blob requests from the Azure Logic Apps runtime. |
| `Runtime.DataStorage.RequestOptionsMaximumExecutionTime` | `00:02:00` <br>(2 min) | Sets the operation timeout value, including retries, for table and queue storage requests from the Azure Logic Apps runtime. |
| `Runtime.ContentStorage.RequestOptionsDeltaBackoff` | `00:00:02` <br>(2 sec) | Sets the backoff interval between retries sent to blob storage. |
| `Runtime.ContentStorage.RequestOptionsMaximumAttempts` | `4` retries | Sets the maximum number of retries sent to table and queue storage. |
||||

<a name="store-inline-or-blob"></a>

### Store content inline or use blobs

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.FlowRunEngine.ForeachMaximumItemsForContentInlining` | `20` items | When a `For each` loop is running, each item's value is stored either inline with other metadata in table storage or separately in blob storage. Sets the number of items to store inline with other metadata. |
| `Runtime.FlowRunRetryableActionJobCallback.MaximumPagesForContentInlining` | `20` pages | Sets the maximum number of pages to store as inline content in table storage before storing in blob storage. |
| `Runtime.FlowTriggerSplitOnJob.MaximumItemsForContentInlining` | `40` items | When the `SplitOn` setting debatches array items into multiple workflow instances, each item's value is stored either inline with other metadata in table storage or separately in blob storage. Sets the number of items to store inline. |
| `Runtime.ScaleUnit.MaximumCharactersForContentInlining` | `8192` characters | Sets the maximum number of operation input and output characters to store inline in table storage before storing in blob storage. |
||||

<a name="table-queue-storage"></a>

### Table and queue storage

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.DataStorage.RequestOptionsServerTimeout` | `00:00:16` <br>(16 sec) | Sets the timeout value for table and queue storage requests from the Azure Logic Apps runtime. |
| `Runtime.DataStorage.RequestOptionsMaximumExecutionTime` | `00:00:45` <br>(45 sec) | Sets the operation timeout value, including retries, for table and queue storage requests from the Azure Logic Apps runtime. |
| `Runtime.DataStorage.RequestOptionsDeltaBackoff` | `00:00:02` <br>(2 sec) | Sets the backoff interval between retries sent to table and queue storage. |
| `Runtime.DataStorage.RequestOptionsMaximumAttempts` | `4` retries | Sets the maximum number of retries sent to table and queue storage. |
||||

<a name="retry-policy"></a>

### Retry policy for all other operations

| Setting | Default value | Description |
|---------|---------------|-------------|
| `Runtime.ScaleMonitor.MaxPollingLatency` | `00:00:30` <br>(30 sec) | Sets the maximum polling latency for runtime scaling. |
| `Runtime.Backend.Operation.MaximumRetryCount` | `90` retries | Sets the maximum number of retries in the retry policy definition for a workflow operation. |
| `Runtime.Backend.Operation.MaximumRetryInterval` | `01:00:00:01` <br>(1 day and 1 sec) | Sets the maximum interval in the retry policy definition for a workflow operation. |
| `Runtime.Backend.Operation.MinimumRetryInterval` | `00:00:05` <br>(5 sec) | Sets the minimum interval in the retry policy definition for a workflow operation. |
||||

<a name="manage-host-settings"></a>

## Manage host settings - host.json

You can add, update, or delete host settings, which specify the runtime configuration settings and values that apply to *all the workflows* in that logic app, such as default values for throughput, capacity, data size, and so on, *whether they run locally or in Azure*. For host settings specific to logic apps, review the [reference guide for available runtime and deployment settings - host.json](#reference-host-json).

### Visual Studio Code - host.json

To review the host settings for your logic app in Visual Studio Code, follow these steps:

1. In your logic app project, at the root project level, find and open the **host.json** file.

1. In the `extensions` object, under `workflows` and `settings`, review any host settings that were previously added for your logic app. Otherwise, the `extensions` object won't appear in the file.

   For more information about host settings, review the [reference guide for available host settings - host.json](#reference-host-json).

To add a host setting, follow these steps:

1. In the **host.json** file, under the `extensionBundle` object, add the `extensions` object, which includes the `workflow` and `settings` objects, for example:

   ```json
   {
      "version": "2.0",
      "extensionBundle": {
         "id": "Microsoft.Azure.Functions.ExtensionBundle",
         "version": "[1.*, 2.0.0)"
      },
      "extensions": {
         "workflow": {
            "settings": {
            }
         }
      }
   }
   ```

1. In the `settings` object, add a flat list with the host settings that you want to use for all the workflows in your logic app, whether those workflows run locally or in Azure, for example:

   ```json
   {
      "version": "2.0",
      "extensionBundle": {
         "id": "Microsoft.Azure.Functions.ExtensionBundle",
         "version": "[1.*, 2.0.0)"
      },
      "extensions": {
         "workflow": {
            "settings": {
               "Runtime.Trigger.MaximumWaitingRuns": "100"
            }
         }
      }
   }
   ```

### Azure portal - host.json

To review the host settings for your single-tenant based logic app in the Azure portal, follow these steps:

1. In the [Azure portal](https://portal.azure.com/) search box, find and open your logic app.

1. On your logic app menu, under **Development Tools**, select **Advanced Tools**.

1. On the **Advanced Tools** page, select **Go**, which opens the **Kudu** environment for your logic app.

1. On the Kudu toolbar, from the **Debug console** menu, select **CMD**.

1. In the Azure portal, stop your logic app.

   1. On your logic app menu, select **Overview**.

   1. On the **Overview** pane's toolbar, select **Stop**.

1. On your logic app menu, under **Development Tools**, select **Advanced Tools**.

1. On the **Advanced Tools** pane, select **Go**, which opens the Kudu environment for your logic app.

1. On the Kudu toolbar, open the **Debug console** menu, and select **CMD**.

   A console window opens so that you can browse to the **wwwroot** folder using the command prompt. Or, you can browse the directory structure that appears above the console window.

1. Browse along the following path to the **wwwroot** folder: `...\home\site\wwwroot`.

1. Above the console window, in the directory table, next to the **host.json** file, select **Edit**.

1. After the **host.json** file opens, review any host settings that were previously added for your logic app.

   For more information about host settings, review the [reference guide for available host settings - host.json](#reference-host-json).

To add a setting, follow these steps:

1. Before you add or edit settings, stop your logic app in the Azure portal.

   1. On your logic app menu, select **Overview**.
   1. On the **Overview** pane's toolbar, select **Stop**.

1. Return to the **host.json** file. Under the `extensionBundle` object, add the `extensions` object, which includes the `workflow` and `settings` objects, for example:

   ```json
   {
      "version": "2.0",
      "extensionBundle": {
         "id": "Microsoft.Azure.Functions.ExtensionBundle",
         "version": "[1.*, 2.0.0)"
      },
      "extensions": {
         "workflow": {
            "settings": {
            }
         }
      }
   }
   ```

1. In the `settings` object, add a flat list with the host settings that you want to use for all the workflows in your logic app, whether those workflows run locally or in Azure, for example:

   ```json
   {
      "version": "2.0",
      "extensionBundle": {
         "id": "Microsoft.Azure.Functions.ExtensionBundle",
         "version": "[1.*, 2.0.0)"
      },
      "extensions": {
         "workflow": {
            "settings": {
               "Runtime.Trigger.MaximumWaitingRuns": "100"
            }
         }
      }
   }
   ```

1. When you're done, remember to select **Save**.

1. Now, restart your logic app. Return to your logic app's **Overview** page, and select **Restart**.

---

## Next steps

- [Create parameters for values that change in workflows between environments for single-tenant Azure Logic Apps](parameterize-workflow-app.md)
- [Set up DevOps deployment for single-tenant Azure Logic Apps](set-up-devops-deployment-single-tenant-azure-logic-apps.md)

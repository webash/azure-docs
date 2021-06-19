---
title: 'Quickstart: Create a web app on Azure Arc'
description: Get started with App Service on Azure Arc deploying your first web app.
ms.topic: quickstart
ms.date: 06/02/2021
---

# Create an App Service app on Azure Arc (Preview)

In this quickstart, you create an [App Service app to an Azure Arc enabled Kubernetes cluster](overview-arc-integration.md) (Preview). This scenario supports Linux apps only, and you can use a built-in language stack or a custom container.

## Prerequisites

- [Set up your Azure Arc enabled Kubernetes to run App Service](manage-create-arc-environment.md).

[!INCLUDE [app-service-arc-cli-install-extensions](../../includes/app-service-arc-cli-install-extensions.md)]

## 1. Create a resource group

Run the following command.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## 2. Get the custom location

[!INCLUDE [app-service-arc-get-custom-location](../../includes/app-service-arc-get-custom-location.md)]


## 3. Create an App Service plan

Run the following command replacing `$customLocationId` obtained from the previous step.

```azurecli-interactive
az appservice plan create -g myResourceGroup -n myPlan \
    --custom-location $customLocationId \
    --per-site-scaling --is-linux --sku K1
``` 

## 4. Create an app

The following example creates a Node.js app. Replace `<app-name>` with a name that's unique within your cluster (valid characters are `a-z`, `0-9`, and `-`). To see all supported runtimes, run [`az webapp list-runtimes --linux`](/cli/azure/webapp).

```azurecli-interactive
 az webapp create \
    --plan myPlan
    --resource-group myResourceGroup \
    --name <app-name> \
    --custom-location $customLocationId \
    --runtime 'NODE|12-lts'
```

## 5. Deploy some code

> [!NOTE]
> `az webapp up` is not supported during the public preview.

Get a sample Node.js app using Git and deploy it using [ZIP deploy](deploy-zip.md). Replace `<app-name>` with your web app name.

```azurecli-interactive
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
cd nodejs-docs-hello-world
zip -r package.zip .
az webapp deployment source config-zip --resource-group myResourceGroup --name <app-name> --src package.zip
```

## 6. Get diagnostic logs using Log Analytics

> [!NOTE]
> To use Log Analytics, you should've previously enabled it when [installing the App Service extension](manage-create-arc-environment.md#install-the-app-service-extension). If you installed the extension without Log Analytics, skip this step.

Navigate to the [Log Analytics workspace that's configured with your App Service extension](manage-create-arc-environment.md#install-the-app-service-extension), then click Logs in the left navigation. Run the following sample query to show logs over the past 72 hours. Replace `<app-name>` with your web app name. 

```kusto
let StartTime = ago(72h);
let EndTime = now();
AppServiceConsoleLogs_CL
| where TimeGenerated between (StartTime .. EndTime)
| where AppName_s =~ "<app-name>"
```

The application logs for all the apps hosted in your Kubernetes cluster are logged to the Log Analytics workspace in the custom log table named `AppServiceConsoleLogs_CL`. 

**Log_s** contains application logs for a given App Service and **AppName_s** contains the App Service app name. In addition to logs you write via your application code, the Log_s column also contains logs on container startup, shutdown, and Function Apps.

You can learn more about log queries in [getting started with Kusto](../azure-monitor/logs/get-started-queries.md).

## (Optional) Deploy a custom container

To create a custom container app, run [az webapp create](/cli/azure/webapp#az_webapp_create) with `--deployment-container-image-name`. For a private repository, add `--docker-registry-server-user` and `--docker-registry-server-password`.

For example, try:

```azurecli-interactive
az webapp create 
    --resource-group myResourceGroup \
    --name <app-name> \
    --custom-location $customLocationId \
    --deployment-container-image-name mcr.microsoft.com/appsvc/node:12-lts
```

<!-- `TODO: currently gets an error but the app is successfully created: "Error occurred in request., RetryError: HTTPSConnectionPool(host='management.azure.com', port=443): Max retries exceeded with url: /subscriptions/62f3ac8c-ca8d-407b-abd8-04c5496b2221/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/cephalin-arctest4/config/appsettings?api-version=2020-12-01 (Caused by ResponseError('too many 500 error responses',))"` -->

To update the image after the app is create, see [Change the Docker image of a custom container](configure-custom-container.md?pivots=container-linux#change-the-docker-image-of-a-custom-container)

## Next steps

- [Configure an ASP.NET Core app](configure-language-dotnetcore.md?pivots=platform-linux)
- [Configure a Node.js app](configure-language-nodejs.md?pivots=platform-linux)
- [Configure a PHP app](configure-language-php.md?pivots=platform-linux)
- [Configure a Linux Python app](configure-language-python.md)
- [Configure a Java app](configure-language-java.md?pivots=platform-linux)
- [Configure a Linux Ruby app](configure-language-ruby.md)
- [Configure a custom container](configure-custom-container.md?pivots=container-linux)

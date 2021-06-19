---
title: "Tutorial: Use GitHub Actions to deploy to App Service for Containers and connect to a database"
description: Learn how to deploy an ASP.NET core app to Azure and to Azure SQL Database with GitHub Actions
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/22/2021
ms.author: jukullam
ms.custom: github-actions-azure
---

# Tutorial: Use GitHub Actions to deploy to App Service for Containers and connect to a database

This tutorial walks you through setting up a GitHub Actions workflow to deploy a containerized ASP.NET Core application with an [Azure SQL Database](../azure-sql/database/sql-database-paas-overview.md) backend. When you're finished, you have an ASP.NET app running in Azure and connected to SQL Database. You'll first create Azure resources with an [ARM template](../azure-resource-manager/templates/overview.md) GitHub Actions workflow.

In this tutorial, you learn how to:

> [!div class="checklist"]
>
> - Use a GitHub Actions workflow to add resources to Azure with a Azure Resource Manager template (ARM template)
> - Use a GitHub Actions workflow to build a container with the latest web app changes

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Prerequisites

To complete this tutorial, you'll need:

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- A GitHub account. If you don't have one, sign up for [free](https://github.com/join).
  - A GitHub repository to store your Resource Manager templates and your workflow files. To create one, see [Creating a new repository](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository).

## Download the sample

[Fork the sample project](https://github.com/Azure-Samples/dotnetcore-containerized-sqldb-ghactions/) in the Azure Samples repo.

```
https://github.com/Azure-Samples/dotnetcore-containerized-sqldb-ghactions/
```

## Create the resource group

Open the Azure Cloud Shell at https://shell.azure.com. You can alternately use the Azure CLI if you've installed it locally. (For more information on Cloud Shell, see the Cloud Shell Overview.)

```azurecli-interactive
    az group create --name {resource-group-name} --location {resource-group-location}
```

## Generate deployment credentials

You'll need to authenticate with a service principal for the resource deployment script to work. You can create a [service principal](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) with the [az ad sp create-for-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac) command in the [Azure CLI](/cli/azure/). Run this command with [Azure Cloud Shell](https://shell.azure.com/) in the Azure portal or by selecting the **Try it** button.

```azurecli-interactive
    az ad sp create-for-rbac --name "{service-principal-name}" --sdk-auth --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

In the example, replace the placeholders with your subscription ID, resource group name, and service principal name. The output is a JSON object with the role assignment credentials that provide access to your App Service app. Copy this JSON object for later. For help, go to [configure deployment credentials](https://github.com/Azure/login#configure-deployment-credentials).

```output
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

> [!IMPORTANT]
> It is always a good practice to grant minimum access. The scope in the previous example is limited to the specific App Service app and not the entire resource group.

## Configure the GitHub secret for authentication

In [GitHub](https://github.com/), browse your repository, select **Settings > Secrets > Add a new secret**.

To use [user-level credentials](#generate-deployment-credentials), paste the entire JSON output from the Azure CLI command into the secret's value field. Give the secret the name `AZURE_CREDENTIALS`.

## Add a SQL Server secret

Create a new secret in your repository for `SQL_SERVER_ADMIN_PASSWORD`. This secret can be any password that meets the Azure standards for password security. You won't be able to access this password again so save it separately.

## Create Azure resources

The create Azure resources workflow runs an [ARM template](../azure-resource-manager/templates/overview.md) to deploy resources to Azure. The workflow:

- Checks out source code with the [Checkout action](https://github.com/marketplace/actions/checkout).
- Logs into Azure with the [Azure Login action](https://github.com/marketplace/actions/azure-login) and gathers environment and Azure resource information.
- Deploys resources with the [Azure Resource Manager Deploy action](https://github.com/marketplace/actions/deploy-azure-resource-manager-arm-template).

To run the create Azure resources workflow:

1. Open the `azuredeploy.yaml` file in `.github/workflows` within your repository.

1. Update the value of `AZURE_RESOURCE_GROUP` to your resource group name.

1. Go to **Actions** and select **Run workflow**.

   :::image type="content" source="media/github-actions-workflows/github-actions-run-workflow.png" alt-text="Run the GitHub Actions workflow to add resources.":::

1. Verify that your action ran successfully by checking for a green checkmark on the **Actions** page.

   :::image type="content" source="media/github-actions-workflows/create-resources-success.png" alt-text="Successful run of create resources. ":::

## Add container registry and SQL secrets

1. In the Azure portal, open your newly created Azure Container Registry in your resource group.

1. Go to **Access keys** and copy the username and password values.

1. Create new GitHub secrets for `ACR_USERNAME` and `ACR_PASSWORD` password in your repository.

1. In the Azure portal, open your Azure SQL database. Open **Connection strings** and copy the value.

1. Create a new secret for `SQL_CONNECTION_STRING`. Replace `{your_password}` with your `SQL_SERVER_ADMIN_PASSWORD`.

## Build, push, and deploy your image

The build, push, and deploy workflow builds a container with the latest app changes, pushes the container to [Azure Container Registry](../container-registry/index.yml) and, updates the web application staging slot to point to the latest container pushed. The workflow containers a build and deploy job:

- The build job checks out source code with the [Checkout action](https://github.com/marketplace/actions/checkout). The job then uses the [Docker login action](https://github.com/marketplace/actions/docker-login) and a custom script to authenticate with Azure Container Registry, build a container image, and deploy it to Azure Container Registry.
- The deployment job logs into Azure with the [Azure Login action](https://github.com/marketplace/actions/azure-login) and gathers environment and Azure resource information. The job then updates Web App Settings with the [Azure App Service Settings action](https://github.com/marketplace/actions/azure-app-service-settings) and deploys to an App Service staging slot with the [Azure Web Deploy action](https://github.com/marketplace/actions/azure-webapp). Last, the job runs a custom script to update the SQL database and swaps staging slot to production.

To run the build, push, and deploy workflow:

1. Open your `build-deploy.yaml` file in `.github/workflows` within your repository.

1. Verify that the environment variables for `AZURE_RESOURCE_GROUP` and `WEB_APP_NAME` match the ones in `azuredeploy.yaml`.

1. Update the `ACR_LOGIN_SERVER` value for your Azure Container Registry login server.

## Next steps

> [!div class="nextstepaction"]
> [Learn about Azure and GitHub integration](/azure/developer/github/)
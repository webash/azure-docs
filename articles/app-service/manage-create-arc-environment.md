---
title: 'Set up Azure Arc for App Service, Functions, and Logic Apps'
description: For your Azure Arc enabled Kubernetes clusters, learn how to enable App Service apps, function apps, and logic apps.
ms.topic: article
ms.date: 05/26/2021
---
# Set up an Azure Arc enabled Kubernetes cluster to run App Service, Functions, and Logic Apps (Preview)

If you have an [Azure Arc enabled Kubernetes cluster](../azure-arc/kubernetes/overview.md), you can use it to create an [App Service enabled custom location](overview-arc-integration.md) and deploy web apps, function apps, and logic apps to it.

Azure Arc enabled Kubernetes lets you make your on-premises or cloud Kubernetes cluster visible to App Service, Functions, and Logic Apps in Azure. You can create an app and deploy to it just like another Azure region.

## Prerequisites

If you don't have an Azure account, [sign up today](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension) for a free account.

<!-- ## Prerequisites

- Create a Kubernetes cluster in a supported Kubernetes distribution and connect it to Azure Arc in a supported region. See [Public preview limitations](overview-arc-integration.md#public-preview-limitations).
- [Install Azure CLI](/cli/azure/install-azure-cli), or use the [Azure Cloud Shell](../cloud-shell/overview.md).
- [Install kubectl](https://kubernetes.io/docs/tasks/tools/). It's also preinstalled in the Azure Cloud Shell.

## Obtain cluster information

Set the following environment variables based on your Kubernetes cluster deployment:

```bash
staticIp="<public-ip-address-of-the-kubernetes-cluster>"
aksClusterGroupName="<name-resource-group-with-aks-cluster>"
groupName="<name-of-resource-group-with-the-arc-connected-cluster>"
clusterName="<name-of-arc-connected-cluster>"
geomasterLocation="TODO: Why so many different locations for different resources? Shouldn't we just say create everything in the connected cluster's resource group and location?"
``` -->

## Add Azure CLI extensions

Launch the Bash environment in [Azure Cloud Shell](../cloud-shell/quickstart.md).

[![Launch Cloud Shell in a new window](../../includes/media/cloud-shell-try-it/hdi-launch-cloud-shell.png)](https://shell.azure.com)

Because these CLI commands are not yet part of the core CLI set, add them with the following commands.

```azurecli-interactive
az extension add --upgrade --yes --name connectedk8s
az extension add --upgrade --yes --name k8s-extension
az extension add --upgrade --yes --name customlocation
az provider register --namespace Microsoft.ExtendedLocation --wait
az provider register --namespace Microsoft.Web --wait
az provider register --namespace Microsoft.KubernetesConfiguration --wait
az extension remove --name appservice-kube
az extension add --yes --source "https://aka.ms/appsvc/appservice_kube-latest-py2.py3-none-any.whl"
```

## Create a connected cluster

> [!NOTE]
> This tutorial uses [Azure Kubernetes Service (AKS)](../aks/index.yml) to provide concrete instructions for setting up an environment from scratch. However, for a production workload, you will likely not want to enable Azure Arc on an AKS cluster as it is already managed in Azure. The steps below will help you get started understanding the service, but for production deployments, they should be viewed as illustrative, not prescriptive. See [Quickstart: Connect an existing Kubernetes cluster to Azure Arc](../azure-arc/kubernetes/quickstart-connect-cluster.md) for general instructions on creating an Azure Arc enabled Kubernetes cluster.

1. Create a cluster in Azure Kubernetes Service with a public IP address. Replace `<group-name>` with the resource group name you want.

    ```azurecli-interactive
    aksClusterGroupName="<group-name>" # Name of resource group for the AKS cluster
    aksName="${aksClusterGroupName}-aks" # Name of the AKS cluster
    resourceLocation="eastus" # "eastus" or "westeurope"

    az group create -g $aksClusterGroupName -l $resourceLocation
    az aks create --resource-group $aksClusterGroupName --name $aksName --enable-aad --generate-ssh-keys
    infra_rg=$(az aks show --resource-group $aksClusterGroupName --name $aksName --output tsv --query nodeResourceGroup)
    az network public-ip create --resource-group $infra_rg --name MyPublicIP --sku STANDARD
    staticIp=$(az network public-ip show --resource-group $infra_rg --name MyPublicIP --output tsv --query ipAddress)
    ```
    
2. Get the [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) file and test your connection to the cluster. By default, the kubeconfig file is saved to `~/.kube/config`.

    ```azurecli-interactive
    az aks get-credentials --resource-group $aksClusterGroupName --name $aksName --admin
    
    kubectl get ns
    ```
    
3. Create a resource group to contain your Azure Arc resources. Replace `<group-name>` with the resource group name you want.

    ```azurecli-interactive
    groupName="<group-name>" # Name of resource group for the connected cluster

    az group create -g $groupName -l $resourceLocation
    ```
    
4. Connect the cluster you created to Azure Arc.

    ```azurecli-interactive
    clusterName="${groupName}-cluster" # Name of the connected cluster resource

    az connectedk8s connect --resource-group $groupName --name $clusterName
    ```
    
5. Validate the connection with the following command. It should show the `provisioningState` property as `Succeeded`. If not, run the command again after a minute.

    ```azurecli-interactive
    az connectedk8s show --resource-group $groupName --name $clusterName
    ```
    
## Create a Log Analytics workspace

While a [Log Analytic workspace](../azure-monitor/logs/quick-create-workspace.md) is not required to run App Service in Azure Arc, it's how developers can get application logs for their apps that are running in the Azure Arc enabled Kubernetes cluster. 

1. For simplicity, create the workspace now.

    ```azurecli-interactive
    workspaceName="$groupName-workspace" # Name of the Log Analytics workspace
    
    az monitor log-analytics workspace create \
        --resource-group $groupName \
        --workspace-name $workspaceName
    ```
    
2. Run the following commands to get the encoded workspace ID and shared key for an existing Log Analytics workspace. You need them in the next step.

    ```azurecli-interactive
    logAnalyticsWorkspaceId=$(az monitor log-analytics workspace show \
        --resource-group $groupName \
        --workspace-name $workspaceName \
        --query customerId \
        --output tsv)
    logAnalyticsWorkspaceIdEnc=$(printf %s $logAnalyticsWorkspaceId | base64) # Needed for the next step
    logAnalyticsKey=$(az monitor log-analytics workspace get-shared-keys \
        --resource-group $groupName \
        --workspace-name $workspaceName \
        --query primarySharedKey \
        --output tsv)
    logAnalyticsKeyEncWithSpace=$(printf %s $logAnalyticsKey | base64)
    logAnalyticsKeyEnc=$(echo -n "${logAnalyticsKeyEncWithSpace//[[:space:]]/}") # Needed for the next step
    ```
    
## Install the App Service extension

1. Set the following environment variables for the desired name of the [App Service extension](overview-arc-integration.md), the cluster namespace in which resources should be provisioned, and the name for the App Service Kubernetes environment. Choose a unique name for `<kube-environment-name>`, because it will be part of the domain name for app created in the App Service Kubernetes environment.

    ```bash
    extensionName="appservice-ext" # Name of the App Service extension
    namespace="appservice-ns" # Namespace in your cluster to install the extension and provision resources
    kubeEnvironmentName="<kube-environment-name>" # Name of the App Service Kubernetes environment resource
    ```
    
2. Install the App Service extension to your Azure Arc connected cluster, with Log Analytics enabled. Again, while Log Analytics is not required, you can't add it to the extension later, so it's easier to do it now.

    ```azurecli-interactive
    az k8s-extension create \
        --resource-group $groupName \
        --name $extensionName \
        --cluster-type connectedClusters \
        --cluster-name $clusterName \
        --extension-type 'Microsoft.Web.Appservice' \
        --release-train stable \
        --auto-upgrade-minor-version true \
        --scope cluster \
        --release-namespace $namespace \
        --configuration-settings "Microsoft.CustomLocation.ServiceAccount=default" \
        --configuration-settings "appsNamespace=${namespace}" \
        --configuration-settings "clusterName=${kubeEnvironmentName}" \
        --configuration-settings "loadBalancerIp=${staticIp}" \
        --configuration-settings "keda.enabled=true" \
        --configuration-settings "buildService.storageClassName=default" \
        --configuration-settings "buildService.storageAccessMode=ReadWriteOnce" \
        --configuration-settings "customConfigMap=${namespace}/kube-environment-config" \
        --configuration-settings "envoy.annotations.service.beta.kubernetes.io/azure-load-balancer-resource-group=${aksClusterGroupName}" \
        --configuration-settings "logProcessor.appLogs.destination=log-analytics" \
        --configuration-protected-settings "logProcessor.appLogs.logAnalyticsConfig.customerId=${logAnalyticsWorkspaceIdEnc}" \
        --configuration-protected-settings "logProcessor.appLogs.logAnalyticsConfig.sharedKey=${logAnalyticsKeyEnc}"
    ```

    > [!NOTE]
    > To install the extension without Log Analytics integration, remove the last three `--configuration-settings` parameters from the command.
    >

    The following table describes the various `--configuration-settings` parameters when running the command:
    
    | Parameter | Description |
    | - | - |
    | `Microsoft.CustomLocation.ServiceAccount` | The service account that should be created for the custom location that will be created. It is recommended that this be set to the value `default`. |
    | `appsNamespace` | The namespace to provision the app definitions and pods. Must match that of the extension release namespace. |
    | `clusterName` | The name of the App Service Kubernetes environment that will be created against this extension. |
    | `loadBalancerIp` | The public IP of the Kubernetes cluster. App Service apps receive traffic on this IP address. Also informs default DNS mapping. |
    | `keda.enabled` | Whether [KEDA](https://keda.sh/) should be installed on the Kubernetes cluster. Accepts `true` or `false`. |
    | `buildService.storageClassName` | The [name of the storage class](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#class) for the build service to store build artifacts. A value like `default` specifies a class named `default`, and not [any class that is marked as default](https://kubernetes.io/docs/tasks/administer-cluster/change-default-storage-class/). |
    | `buildService.storageAccessMode` | The [access mode](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes) to use with the named storage class above. Accepts `ReadWriteOnce` or `ReadWriteMany`. |
    | `customConfigMap` | The name of the config map that will be set by the App Service Kubernetes environment. Currently, it must be `<namespace>/kube-environment-config`, replacing `<namespace>` with the value of `appsNamespace` above. |
    | `envoy.annotations.service.beta.kubernetes.io/azure-load-balancer-resource-group` | The name of the resource group in which the Azure Kubernetes Service cluster resides. Valid and required only when the underlying cluster is Azure Kubernetes Service.  |
    | `logProcessor.appLogs.destination` | Optional. Accepts `log-analytics`. |
    | `logProcessor.appLogs.logAnalyticsConfig.customerId` | Required only when `logProcessor.appLogs.destination` is set to `log-analytics`. The base64-encoded Log analytics workspace ID. This parameter should be configured as a protected setting. |
    | `logProcessor.appLogs.logAnalyticsConfig.sharedKey` | Required only when `logProcessor.appLogs.destination` is set to `log-analytics`. The base64-encoded Log analytics workspace shared key. This parameter should be configured as a protected setting. |
    | | |
        
3. Save the `id` property of the App Service extension for later.

    ```azurecli-interactive
    extensionId=$(az k8s-extension show \
        --cluster-type connectedClusters \
        --cluster-name $clusterName \
        --resource-group $groupName \
        --name $extensionName \
        --query id \
        --output tsv)
    ```

4. Wait for the extension to fully install before proceeding. You can have your terminal session wait until this complete by running the following command:

    ```azurecli-interactive
    az resource wait --ids $extensionId --custom "properties.installState!='Pending'" --api-version "2020-07-01-preview"
    ```

You can use `kubectl` to see the pods that have been created in your Kubernetes cluster:

```bash
kubectl get pods -n $namespace
```

You can learn more about these pods and their role in the system from [Pods created by the App Service extension](overview-arc-integration.md#pods-created-by-the-app-service-extension).

## Create a custom location

The [custom location](../azure-arc/kubernetes/custom-locations.md) in Azure is used to assign the App Service Kubernetes environment.

<!-- https://github.com/MicrosoftDocs/azure-docs-pr/pull/156618 -->

1. Set the following environment variables for the desired name of the custom location and for the ID of the Azure Arc connected cluster.

    ```bash
    customLocationName="my-custom-location" # Name of the custom location
    
    connectedClusterId=$(az connectedk8s show --resource-group $groupName --name $clusterName --query id --output tsv)
    ```
    
3. Create the custom location:

    ```azurecli-interactive
    az customlocation create \
        --resource-group $groupName \
        --name $customLocationName \
        --host-resource-id $connectedClusterId \
        --namespace $namespace \
        --cluster-extension-ids $extensionId
    ```
    
    <!-- --kubeconfig ~/.kube/config # needed for non-Azure -->

4. Validate that the custom location is successfully created with the following command. The output should show the `provisioningState` property as `Succeeded`. If not, run it again after a minute.

    ```azurecli-interactive
    az customlocation show \
        --resource-group $groupName \
        --name $customLocationName
    ```
    
5. Save the custom location ID for the next step.

    ```azurecli-interactive
    customLocationId=$(az customlocation show \
        --resource-group $groupName \
        --name $customLocationName \
        --query id \
        --output tsv)
    ```
    
## Create the App Service Kubernetes environment

Before you can start creating apps on the custom location, you need an [App Service Kubernetes environment](overview-arc-integration.md#app-service-kubernetes-environment).

1. Create the App Service Kubernetes environment:

    ```azurecli-interactive
    az appservice kube create \
        --resource-group $groupName \
        --name $kubeEnvironmentName \
        --custom-location $customLocationId \
        --static-ip $staticIp
    ```
    
2. Validate that the App Service Kubernetes environment is successfully created with the following command. The output should show the `provisioningState` property as `Succeeded`. If not, run it again after a minute.

    ```azurecli-interactive
    az appservice kube show \
        --resource-group $groupName \
        --name $kubeEnvironmentName
    ```
    

## Next steps

- [Quickstart: Create a web app on Azure Arc](quickstart-arc.md)
- [Create your first function on Azure Arc](../azure-functions/create-first-function-arc-cli.md)
- [Create your first logic app on Azure Arc](../logic-apps/azure-arc-enabled-logic-apps-create-deploy-workflows.md)
---
title: Assign an Azure role for access to blob data
titleSuffix: Azure Storage
description: Learn how to assign permissions for blob data to an Azure Active Directory security principal with Azure role-based access control (Azure RBAC). Azure Storage supports built-in and Azure custom roles for authentication and authorization via Azure AD.
services: storage
author: tamram

ms.service: storage
ms.topic: how-to
ms.date: 06/07/2021
ms.author: tamram
ms.reviewer: sohamnc
ms.subservice: common 
ms.custom: devx-track-azurepowershell
---

# Assign an Azure role for access to blob data

Azure Active Directory (Azure AD) authorizes access rights to secured resources through [Azure role-based access control (Azure RBAC)](../../role-based-access-control/overview.md). Azure Storage defines a set of Azure built-in roles that encompass common sets of permissions used to access containers.

When an Azure role is assigned to an Azure AD security principal, Azure grants access to those resources for that security principal. Access can be scoped to the level of the subscription, the resource group, the storage account, or an individual container. An Azure AD security principal may be a user, a group, an application service principal, or a [managed identity for Azure resources](../../active-directory/managed-identities-azure-resources/overview.md).

This article shows how to assign Azure roles for data access to blobs.

## Azure roles for blobs

[!INCLUDE [storage-auth-rbac-roles-blob-include](../../../includes/storage-auth-rbac-roles-blob-include.md)]

## Determine resource scope

[!INCLUDE [storage-auth-resource-scope-blob-include](../../../includes/storage-auth-resource-scope-blob-include.md)]

## Assign an Azure role

You can use the Azure portal, PowerShell, or Azure CLI to assign a role for data access.

# [Azure portal](#tab/portal)

To access blob data in the Azure portal with Azure AD credentials, a user must have the following role assignments:

- A data access role, such as **Storage Blob Data Contributor**
- The Azure Resource Manager **Reader** role

To learn how to assign these roles to a user, follow the instructions provided in [Assign Azure roles using the Azure portal](../../role-based-access-control/role-assignments-portal.md).

The [Reader](../../role-based-access-control/built-in-roles.md#reader) role is an Azure Resource Manager role that permits users to view storage account resources, but not modify them. It does not provide read permissions to data in Azure Storage, but only to account management resources. The **Reader** role is necessary so that users can navigate to blob containers in the Azure portal.

For example, if you assign the **Storage Blob Data Contributor** role to user Mary at the level of a container named **sample-container**, then Mary is granted read, write, and delete access to all of the blobs in that container. However, if Mary wants to view a blob in the Azure portal, then the **Storage Blob Data Contributor** role by itself will not provide sufficient permissions to navigate through the portal to the blob in order to view it. The additional permissions are required to navigate through the portal and view the other resources that are visible there.

A user must be assigned the **Reader** role to use the Azure portal with Azure AD credentials. However, if a user has been assigned a role with **Microsoft.Storage/storageAccounts/listKeys/action** permissions, then the user can use the portal with the storage account keys, via Shared Key authorization. To use the storage account keys, Shared Key access must be permitted for the storage account. For more information on permitting or disallowing Shared Key access, see [Prevent Shared Key authorization for an Azure Storage account](../common/shared-key-authorization-prevent.md).

You can also assign an Azure Resource Manager role that provides additional permissions beyond than the **Reader** role. Assigning the least possible permissions is recommended as a security best practice. For more information, see [Best practices for Azure RBAC](../../role-based-access-control/best-practices.md).

> [!NOTE]
> Prior to assigning yourself a role for data access, you will be able to access data in your storage account via the Azure portal because the Azure portal can also use the account key for data access. For more information, see [Choose how to authorize access to blob data in the Azure portal](../blobs/authorize-data-operations-portal.md).
>
> The preview version of Storage Explorer in the Azure portal does not support using Azure AD credentials to view and modify blob data. Storage Explorer in the Azure portal always uses the account keys to access data. To use Storage Explorer in the Azure portal, you must be assigned a role that includes **Microsoft.Storage/storageAccounts/listkeys/action**.

# [PowerShell](#tab/powershell)

To assign an Azure role to a security principal, call the [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) command. The format of the command can differ based on the scope of the assignment. In order to run the command, you must have a role that includes **Microsoft.Authorization/roleAssignments/write** permissions assigned to you at the corresponding scope or above.

To assign a role scoped to a container, specify a string containing the scope of the container for the `--scope` parameter. The scope for a container is in the form:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

The following example assigns the **Storage Blob Data Contributor** role to a user, scoped to a container named *sample-container*. Make sure to replace the sample values and the placeholder values in brackets with your own values: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

For information about assigning roles with PowerShell at the subscription, resource group, or storage account scope, see [Assign Azure roles using Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

# [Azure CLI](#tab/azure-cli)

To assign an Azure role to a security principal, use the [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create) command. The format of the command can differ based on the scope of the assignment. The format of the command can differ based on the scope of the assignment. In order to run the command, you must have a role that includes **Microsoft.Authorization/roleAssignments/write** permissions assigned to you at the corresponding scope or above.

To assign a role scoped to a container, specify a string containing the scope of the container for the `--scope` parameter. The scope for a container is in the form:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

The following example assigns the **Storage Blob Data Contributor** role to a user, scoped to the level of the container. Make sure to replace the sample values and the placeholder values in brackets with your own values:

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container>"
```

For information about assigning roles with PowerShell at the subscription, resource group, or storage account scope, see [Assign Azure roles using Azure CLI](../../role-based-access-control/role-assignments-cli.md).

---

Keep in mind the following points about Azure role assignments in Azure Storage:

- When you create an Azure Storage account, you are not automatically assigned permissions to access data via Azure AD. You must explicitly assign yourself an Azure role for Azure Storage. You can assign it at the level of your subscription, resource group, storage account, or container.
- If the storage account is locked with an Azure Resource Manager read-only lock, then the lock prevents the assignment of Azure roles that are scoped to the storage account or a container.

## Next steps

- [What is Azure role-based access control (Azure RBAC)?](../../role-based-access-control/overview.md)
- [Best practices for Azure RBAC](../../role-based-access-control/best-practices.md)

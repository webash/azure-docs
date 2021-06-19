---
title: View audit log report for Azure AD roles in Azure AD PIM | Microsoft Docs
description: Learn how to view the audit log history for Azure AD roles in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''

ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 06/03/2021
ms.author: curtand
ms.custom: pim

ms.collection: M365-identity-device-management
---
# View audit history for Azure AD roles in Privileged Identity Management

You can use the Privileged Identity Management (PIM) audit history to see all role assignments and activations within the past 30 days for all privileged roles. If you want to retain audit data for longer than the default retention period, you can use Azure Monitor to route it to an Azure storage account. For more information, see [Archive Azure AD logs to an Azure storage account](../reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md). If you want to see the full audit history of activity in your Azure Active Directory (Azure AD) organization, including administrator, end user, and synchronization activity, you can use the [Azure Active Directory security and activity reports](../reports-monitoring/overview-reports.md).

Follow these steps to view the audit history for Azure AD roles.

## View resource audit history

Resource audit gives you a view of all activity associated with your Azure AD roles.

1. Open **Azure AD Privileged Identity Management**.

1. Select **Azure AD roles**.

1. Select **Resource audit**.

1. Filter the history using a predefined date or custom range.

    ![Resource audit list with filters](media/azure-pim-resource-rbac/rbac-resource-audit.png)

## View my audit

My audit enables you to view your personal role activity.

1. Open **Azure AD Privileged Identity Management**.

1. Select **Azure AD roles**.

1. Select the resource you want to view audit history for.

1. Select **My audit**.

1. Filter the history using a predefined date or custom range.

    ![Audit list for the current user](media/azure-pim-resource-rbac/my-audit-time.png)

## Next steps

- [View activity and audit history for Azure resource roles in Privileged Identity Management](azure-pim-resource-rbac.md)

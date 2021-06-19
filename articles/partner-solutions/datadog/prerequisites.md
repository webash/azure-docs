---
title: Prerequisites for Datadog on Azure - Azure partner solutions
description: This article describes how to configure your Azure environment to create an instance of Datadog.
ms.service: partner-services
ms.topic: conceptual
ms.date: 05/28/2021
author: tfitzmac
ms.author: tomfitz
---

# Configure environment before Datadog deployment

This article describes how to set up your environment before deploying your first instance of Datadog. These conditions are prerequisites for completing the quickstarts.

## Access control

To set up the Azure Datadog integration, you must have **Owner** access on the Azure subscription. [Confirm that you have the appropriate access](../../role-based-access-control/check-access.md) before starting the setup.

## Add enterprise application
 
To use the Security Assertion Markup Language (SAML) Single Sign-On (SSO) feature within the Datadog resource, you must set up an enterprise application. To add an enterprise application, you need one of these roles: Global Administrator, Cloud Application Administrator, Application Administrator, or owner of the service principal.

Use the following steps to set up the enterprise application:

1. Go to [Azure portal](https://portal.azure.com). Select **Azure Active Directory**.
1. In the left pane, select **Enterprise applications**.
1. Select **New Application**.
1. In **Add from the gallery**, search for *Datadog*. Select the search result then select **Add**.

   :::image type="content" source="media/prerequisites/datadog-azure-ad-app-gallery.png" alt-text="Datadog application in the AAD enterprise gallery." border="true":::

1. Once the app is created, go to properties from the side panel. Set **User assignment required?** to **No**, and select **Save**.

   :::image type="content" source="media/prerequisites/user-assignment-required.png" alt-text="Set properties for the Datadog application" border="true":::

1. Go to **Single sign-on** from the side panel. Then select **SAML**.

   :::image type="content" source="media/prerequisites/saml-sso.png" alt-text="SAML authentication." border="true":::

1. Select **Yes** when prompted to **Save single sign-on settings**.

   :::image type="content" source="media/prerequisites/save-sso.png" alt-text="Save single-sign on for the Datadog app" border="true":::

1. The setup of single sign-on is now complete.

## Next steps

To create an instance of Datadog, see [QuickStart: Get started with Datadog](create.md).

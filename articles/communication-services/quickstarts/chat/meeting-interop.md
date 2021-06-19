---
title: Getting started with Teams interop on Azure Communication Services
titleSuffix: An Azure Communication Services quickstart
description: In this quickstart, you'll learn how to join a Teams meeting with the Azure Communication Chat SDK
author: askaur
ms.author: askaur
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-web-ios

---

# Quickstart: Join your chat app to a Teams meeting

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include-chat.md)]

> [!IMPORTANT]
> To enable/disable [Teams tenant interoperability](../../concepts/teams-interop.md), complete [this form](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u).

Get started with Azure Communication Services by connecting your chat solution to Microsoft Teams. 

::: zone pivot="platform-web"
[!INCLUDE [Teams interop with JavaScript SDK](./includes/meeting-interop-javascript.md)]
::: zone-end

::: zone pivot="platform-ios"
[!INCLUDE [Teams interop with iOS SDK](./includes/meeting-interop-swift.md)]
::: zone-end

## Clean up resources

If you want to clean up and remove a Communication Services subscription, you can delete the resource or resource group. Deleting the resource group also deletes any other resources associated with it. Learn more about [cleaning up resources](../create-communication-resource.md#clean-up-resources).

## Next steps

For more information, see the following articles:

- Check out our [chat hero sample](../../samples/chat-hero-sample.md)
- Learn more about [how chat works](../../concepts/chat/concepts.md)
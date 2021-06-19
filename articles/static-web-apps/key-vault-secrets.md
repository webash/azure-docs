---
title: Securing authentication secrets in Azure Key Vault
description: Use managed identity to secure authentication secrets in Azure Key Vault.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: how-to
ms.date: 05/17/2021
ms.author: cshoe
---

# Securing authentication secrets in Azure Key Vault

When configuring custom authentication providers, you may want to store connection secrets in Azure Key Vault. This article demonstrates how to use a managed identity to grant Azure Static Web Apps access to Key Vault for custom authentication secrets.

Security secrets require the following items to be in place.

- Create a system-assigned identity in the Static Web Apps instance.
- Grant access a Key Vault secret access to the identity.
- Reference the Key Vault secret from the Static Web Apps application settings.

This article demonstrates how to set up each of these items in your application.

## Prerequisites

- Existing Azure Static Web Apps site.
- Existing Key Vault resource with a secret value.

## Create identity

1. Open your Static Web Apps site in the Azure portal.

1. Under _Settings_ menu, select **Identity**.

1. Select the **System assigned** tab.

1. Under the _Status_ label, select the **On** button.

1. Select the **Save** button.

    :::image type="content" source="media/key-vault-secrets/azure-static-web-apps-enable-managed-identity.png" alt-text="Add system-assigned identity":::

1. When the confirmation dialog appears, select the **Yes** button.

    :::image type="content" source="media/key-vault-secrets/azure-static-web-apps-enable-managed-identity-confirmation.png" alt-text="Confirm identity assignment.":::

You can now add an access policy to allow your static web app to read Key Vault secrets.

## Add a Key Vault access policy

1. Open your Key Vault resource in the Azure portal.

1. Under the _Settings_ menu, select **Access policies**.

1. Select the link, **Add Access Policy**.

1. From the _Secret permissions_ drop down, select **Get**.

1. Next to the _Select principal_ label, select the **None selected** link.

1. In search box, search for your Static Web Apps application name.

1. Select list item that matches your application name.

1. Select the **Select** button.

1. Select the **Add** button.

1. Select the **Save** button.

    :::image type="content" source="media/key-vault-secrets/azure-static-web-apps-key-vault-save-policy.png" alt-text="Save Key Vault access policy":::

The access policy is now saved to Key Vault. Next, access the secret's URI to use when associating your static web app to the Key Vault resource.

1. Under the _Settings_ menu, select **Secrets**.

1. Select your desired secret from the list.

1. Select your desired secret version from the list.

1. Select the **copy button** at end of _Secret Identifier_ text box to copy the secret URI value to the clipboard.

1. Paste this value into a text editor for later use.

## Add application setting

1. Open your Static Web Apps site in the Azure portal.

1. Under the _Settings_ menu, select **Configuration**.

1. Under the _Application settings_ section, select the **Add** button.

1. Enter a name in the text box for the _Name_ field.

1. Determine the secret value in text box for the _Value_ field.

    The secret value is a composite of a few different values. The following template shows how the final string is built.

    ```text
    @Microsoft.KeyVault(SecretUri=<YOUR-KEY-VAULT-SECRET-URI>)
    ```

    Use the following steps to build the full secret value.

1. Copy the template from above and paste it into a text editor.

1. Replace `<YOUR-KEY-VAULT-SECRET-URI>` with the Key Vault URI value you set aside earlier.

1. Copy the new full string value.

1. Paste the value into the text box for the _Value_ field.

1. Select the **OK** button.

1. Select the **Save** button at the top of the _Application settings_ toolbar.

    :::image type="content" source="media/key-vault-secrets/azure-static-web-apps-application-settings-save.png" alt-text="Save application settings":::

Now when your custom authentication configuration references your newly created application setting, the value is extracted from Azure Key Vault using your static web app's identity.

## Next Steps

> [!div class="nextstepaction"]
> [Custom authentication](./authentication-custom.md)

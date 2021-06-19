---
title: Passwordless security key sign-in - Azure Active Directory
description: Enable passwordless security key sign-in to Azure AD using FIDO2 security keys

services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 06/03/2021

ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: librown, aakapo

ms.collection: M365-identity-device-management
---
# Enable passwordless security key sign-in 

For enterprises that use passwords today and have a shared PC environment, security keys provide a seamless way for workers to authenticate without entering a username or password. Security keys provide improved productivity for workers, and have better security.

This document focuses on enabling security key based passwordless authentication. At the end of this article, you will be able to sign in to web-based applications with your Azure AD account using a FIDO2 security key.

## Requirements

- [Azure AD Multi-Factor Authentication](howto-mfa-getstarted.md)
- Enable [Combined security information registration](concept-registration-mfa-sspr-combined.md)
- Compatible [FIDO2 security keys](concept-authentication-passwordless.md#fido2-security-keys)
- WebAuthN requires Windows 10 version 1903 or higher**

To use security keys for logging in to web apps and services, you must have a browser that supports the WebAuthN protocol. 
These include Microsoft Edge, Chrome, Firefox, and Safari.

## Prepare devices

For Azure AD joined devices the best experience is on Windows 10 version 1903 or higher.

Hybrid Azure AD joined devices must run Windows 10 version 2004 or higher.

## Enable passwordless authentication method

### Enable the combined registration experience

Registration features for passwordless authentication methods rely on the combined registration feature. Follow the steps in the article [Enable combined security information registration](howto-registration-mfa-sspr-combined.md), to enable combined registration.

### Enable FIDO2 security key method

1. Sign in to the [Azure portal](https://portal.azure.com).
1. Browse to **Azure Active Directory** > **Security** > **Authentication methods** > **Authentication method policy**.
1. Under the method **FIDO2 Security Key**, choose the following options:
   1. **Enable** - Yes or No
   1. **Target** - All users or Select users
1. **Save** the configuration.


### FIDO Security Key optional settings 

There are some optional settings for managing security keys per tenant.  

![Screenshot of FIDO2 security key options](media/howto-authentication-passwordless-security-key/optional-settings.png) 

**General**

- **Allow self-service set up** should remain set to **Yes**. If set to no, your users will not be able to register a FIDO key through the MySecurityInfo portal, even if enabled by Authentication Methods policy.  
- **Enforce attestation** setting to **Yes** requires the FIDO security key metadata to be published and verified with the FIDO Alliance Metadata Service, and also pass Microsoft’s additional set of validation testing. For more information, see [What is a Microsoft-compatible security key?](/windows/security/identity-protection/hello-for-business/microsoft-compatible-security-key)

**Key Restriction Policy**

- **Enforce key restrictions** should be set to **Yes** only if your organization wants to only allow or disallow certain FIDO security keys, which are identified by their AAGuids. You can work with your security key provider to determine the AAGuids of their devices. If the key is already registered, AAGUID can also be found by viewing the authentication method details of the key per user. 


## Disable a key 

To remove a FIDO2 key associated with a user account, delete the key from the user’s authentication method.

1. Login to the Azure AD portal and search for the user account from which the FIDO key is to be removed.
1. Select **Authentication methods** > right-click **FIDO2 security key** and click **Delete**. 

    ![View Authentication Method details](media/howto-authentication-passwordless-deployment/security-key-view-details.png)

## Security key Authenticator Attestation GUID (AAGUID)

The FIDO2 specification requires each security key provider to provide an Authenticator Attestation GUID (AAGUID) during attestation. An AAGUID is a 128-bit identifier indicating the key type, such as the make and model. 

>[!NOTE]
>The manufacturer must ensure that the AAGUID is identical across all substantially identical keys made by that manufacturer, and different (with high probability) from the AAGUIDs of all other types of keys. To ensure, the AAGUID for a given type of security key should be randomly generated. For more information, see [Web Authentication: An API for accessing Public Key Credentials - Level 2 (w3.org)](https://w3c.github.io/webauthn/).

There are two ways to get your AAGUID. You can either ask your security key provider or view the authentication method details of the key per user.

![View AAGUID for security key](media/howto-authentication-passwordless-deployment/security-key-aaguid-details.png)

## User registration and management of FIDO2 security keys

1. Browse to [https://myprofile.microsoft.com](https://myprofile.microsoft.com).
1. Sign in if not already.
1. Click **Security Info**.
   1. If the user already has at least one Azure AD Multi-Factor Authentication method registered, they can immediately register a FIDO2 security key.
   1. If they don't have at least one Azure AD Multi-Factor Authentication method registered, they must add one.
1. Add a FIDO2 Security key by clicking **Add method** and choosing **Security key**.
1. Choose **USB device** or **NFC device**.
1. Have your key ready and choose **Next**.
1. A box will appear and ask the user to create/enter a PIN for your security key, then perform the required gesture for the key, either biometric or touch.
1. The user will be returned to the combined registration experience and asked to provide a meaningful name for the key so the user can identify which one if they have multiple. Click **Next**.
1. Click **Done** to complete the process.

## Sign in with passwordless credential

In the example below a user has already provisioned their FIDO2 security key. The user can choose to sign in on the web with their FIDO2 security key inside of a supported browser on Windows 10 version 1903 or higher.

![Security key sign-in Microsoft Edge](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-edge-sign-in.png)

## Troubleshooting and feedback

If you'd like to share feedback or encounter issues with this feature, share via the Windows Feedback Hub app using the following steps:

1. Launch **Feedback Hub** and make sure you're signed in.
1. Submit feedback under the following categorization:
   - Category: Security and Privacy
   - Subcategory: FIDO
1. To capture logs, use the option to **Recreate my Problem**.

## Known issues

### Security key provisioning

Administrator provisioning and de-provisioning of security keys is not available.


### UPN changes

If a user's UPN changes, you can no longer modify FIDO2 security keys to account for the change. The solution for a user with a FIDO2 security key is to login to MySecurityInfo, delete the old key, and add a new one.

## Next steps

[FIDO2 security key Windows 10 sign in](howto-authentication-passwordless-security-key-windows.md)

[Enable FIDO2 authentication to on-premises resources](howto-authentication-passwordless-security-key-on-premises.md)

[Learn more about device registration](../devices/overview.md)

[Learn more about Azure AD Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
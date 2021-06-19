---
title: Self-service sign-up for email-verified users - Azure AD | Microsoft Docs
description: Use self-service sign-up in an Azure Active Directory (Azure AD) organization
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''

ms.service: active-directory
ms.subservice: enterprise-users
ms.topic: overview
ms.workload: identity
ms.date: 05/19/2021
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro

ms.collection: M365-identity-device-management
---
# What is self-service sign-up for Azure Active Directory?

This article explains how to use self-service sign-up to populate an organization in Azure Active Directory (Azure AD). If you want to take over a domain name from an unmanaged Azure AD organization, see [Take over an unmanaged tenant as administrator](domains-admin-takeover.md).

## Why use self-service sign-up?

* Get customers to services they want faster
* Create email-based offers for a service
* Create email-based sign-up flows that quickly allow users to create identities using their easy-to-remember work email aliases
* A self-service-created Azure AD tenant can be turned into a managed tenant that can be used for other services

## Terms and definitions

* **Self-service sign-up**: This is the method by which a user signs up for a cloud service and has an identity automatically created for them in Azure AD based on their email domain.
* **Unmanaged Azure AD tenant**: This is the tenant where that identity is created. An unmanaged tenant is a tenant that has no global administrator.
* **Email-verified user**: This is a type of user account in Azure AD. A user who has an identity created automatically after signing up for a self-service offer is known as an email-verified user. An email-verified user is a regular member of a tenant tagged with creationmethod=EmailVerified.

## How do I control self-service settings?

Admins have two self-service controls today. They can control whether:

* Users can join the tenant via email
* Users can license themselves for applications and services

### How can I control these capabilities?

An admin can configure these capabilities using the following Azure AD cmdlet Set-MsolCompanySettings parameters:

* **AllowEmailVerifiedUsers** controls whether users can join the tenant by email validation. To join, the user must have an email address in a domain which matches one of the verified domains in the tenant. This setting is applied company-wide for all domains in the tenant. If you set that parameter to $false, no email-verified user can join the tenant.
* **AllowAdHocSubscriptions** controls the ability for users to perform self-service sign-up. If you set that parameter to $false, no user can perform self-service sign-up.
  
AllowEmailVerifiedUsers and AllowAdHocSubscriptions are tenant-wide settings that can be applied to a managed or unmanaged tenant. Here's an example where:

* You administer a tenant with a verified domain such as contoso.com
* You use B2B collaboration from a different tenant to invite a user that does not already exist (userdoesnotexist@contoso.com) in the home tenant of contoso.com
* The home tenant has the AllowEmailVerifiedUsers turned on

If the preceding conditions are true, then a member user is created in the home tenant, and a B2B guest user is created in the inviting tenant.

For more information on Flow and PowerApps trial sign-ups, see the following articles:

* [How can I prevent my existing users from starting to use Power BI?](https://support.office.com/article/Power-BI-in-your-Organization-d7941332-8aec-4e5e-87e8-92073ce73dc5#bkmk_preventjoining)
* [Flow in your organization Q&A](/flow/organization-q-and-a)

### How do the controls work together?
These two parameters can be used in conjunction to define more precise control over self-service sign-up. For example, the following command will allow users to perform self-service sign-up, but only if those users already have an account in Azure AD (in other words, users who would need an email-verified account to be created first cannot perform self-service sign-up):

```powershell
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
```

The following flowchart explains the different combinations for these parameters and the resulting conditions for the tenant and self-service sign-up.

![flowchart of self-service sign-up controls](./media/directory-self-service-signup/SelfServiceSignUpControls.png)

The details of this setting can be retrieved by the following PowerShell cmdlet Get-MsolCompanyInformation. For more information on this, see [Get-MsolCompanyInformation](/powershell/module/msonline/get-msolcompanyinformation).

```powershell
    Get-MsolCompanyInformation | Select AllowEmailVerifiedUsers, AllowAdHocSubscriptions
```

For more information and examples of how to use these parameters, see [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings).

## Next steps

* [Add a custom domain name to Azure AD](../fundamentals/add-custom-domain.md)
* [How to install and configure Azure PowerShell](/powershell/azure/)
* [Azure PowerShell](/powershell/azure/)
* [Azure Cmdlet Reference](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings)
* [Close your work or school account in an unmanaged tenant](users-close-account.md)

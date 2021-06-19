---
title: 'Tutorial: Azure Active Directory single sign-on (SSO) integration with Discovery Benefits SSO | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Discovery Benefits SSO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/15/2021
ms.author: jeedes
---

# Tutorial: Azure Active Directory single sign-on (SSO) integration with Discovery Benefits SSO

In this tutorial, you'll learn how to integrate Discovery Benefits SSO with Azure Active Directory (Azure AD). When you integrate Discovery Benefits SSO with Azure AD, you can:

* Control in Azure AD who has access to Discovery Benefits SSO.
* Enable your users to be automatically signed-in to Discovery Benefits SSO with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Discovery Benefits SSO single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD SSO in a test environment.

* Discovery Benefits SSO supports **IDP** initiated SSO.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add Discovery Benefits SSO from the gallery

To configure the integration of Discovery Benefits SSO into Azure AD, you need to add Discovery Benefits SSO from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Discovery Benefits SSO** in the search box.
1. Select **Discovery Benefits SSO** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Discovery Benefits SSO

Configure and test Azure AD SSO with Discovery Benefits SSO using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Discovery Benefits SSO.

To configure and test Azure AD SSO with Discovery Benefits SSO, complete the following building blocks:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Discovery Benefits SSO](#configure-discovery-benefits-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Discovery Benefits SSO test user](#create-discovery-benefits-sso-test-user)** - to have a counterpart of B.Simon in Discovery Benefits SSO that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Discovery Benefits SSO** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

1. On the **Basic SAML Configuration** section the application is pre-configured in **IDP** initiated mode and the necessary URLs are already pre-populated with Azure. The user needs to save the configuration by clicking the **Save** button.

1. Discovery Benefits SSO application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration. The following screenshot shows the list of default attributes. Click **Edit** icon to open User Attributes dialog.

	![image](common/edit-attribute.png)

	a. Click on **Edit**  icon to open the **Unique User Identifier (Name ID)** dialog.

	![Screenshot that shows the "User Attributes & Claims" section with the "Required claim" ellipses on the right side selected.](./media/discovery-benefits-sso-tutorial/user-attribute.png)

	![Discovery Benefits SSO configuration](./media/discovery-benefits-sso-tutorial/add-attribute.png)

	b. Click on **Edit** icon to open the **Manage transformation** dialog.

	c. In the **Transformation** textbox, type the **ToUppercase()** shown for that row.

	d. In the **Parameter 1** textbox, type the parameter like `<Name Identifier value>`.

	e. Click **Add**.

	> [!NOTE]
	> Discovery Benefits SSO requires a fixed string value to be passed in **Unique User Identifier (Name ID)** field to get this integration working. Azure AD currently doesn't support this feature so as a work around, you can use **ToUpper** or **ToLower** transformations of NameID to set a fixed string value as shown above in the screenshot.

	f. We have auto-populated the additional claims which are required for SSO configuration (`SSOInstance` and `SSOID`). Use the **pencil** icon to map the values as per your organization.

	![Screenshot that shows the "User Attributes & Claims" with the "S S O Instance" and "S S O I D" values highlighted.](./media/discovery-benefits-sso-tutorial/new-attribute.png)

1. On the **Set up single sign-on with SAML** page, in the **SAML Signing Certificate** section,  find **Certificate (Base64)** and select **Download** to download the certificate and save it on your computer.

	![The Certificate download link](common/certificatebase64.png)

1. On the **Set up Discovery Benefits SSO** section, copy the appropriate URL(s) based on your requirement.

	![Copy configuration URLs](common/copy-configuration-urls.png)

### Create an Azure AD test user

In this section, you'll create a test user in the Azure portal called B.Simon.

1. From the left pane in the Azure portal, select **Azure Active Directory**, select **Users**, and then select **All users**.
1. Select **New user** at the top of the screen.
1. In the **User** properties, follow these steps:
   1. In the **Name** field, enter `B.Simon`.  
   1. In the **User name** field, enter the username@companydomain.extension. For example, `B.Simon@contoso.com`.
   1. Select the **Show password** check box, and then write down the value that's displayed in the **Password** box.
   1. Click **Create**.

### Assign the Azure AD test user

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Discovery Benefits SSO.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Discovery Benefits SSO**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Discovery Benefits SSO

To configure single sign-on on **Discovery Benefits SSO** side, you need to send the downloaded **Certificate (Base64)** and appropriate copied URLs from Azure portal to [Discovery Benefits SSO support team](mailto:Jsimpson@DiscoveryBenefits.com). They set this setting to have the SAML SSO connection set properly on both sides.

### Create Discovery Benefits SSO test user

In this section, you create a user called Britta Simon in Discovery Benefits SSO. Work with [Discovery Benefits SSO support team](mailto:Jsimpson@DiscoveryBenefits.com) to add the users in the Discovery Benefits SSO platform. Users must be created and activated before you use single sign-on.

## Test SSO 

In this section, you test your Azure AD single sign-on configuration with following options.

* Click on Test this application in Azure portal and you should be automatically signed in to the Discovery Benefits SSO for which you set up the SSO.

* You can use Microsoft My Apps. When you click the Discovery Benefits SSO tile in the My Apps, you should be automatically signed in to the Discovery Benefits SSO for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Discovery Benefits SSO you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

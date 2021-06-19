---
title: 'Tutorial: Azure Active Directory integration with SpaceIQ | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and SpaceIQ.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/11/2021
ms.author: jeedes
---
# Tutorial: Azure Active Directory integration with SpaceIQ

In this tutorial, you'll learn how to integrate SpaceIQ with Azure Active Directory (Azure AD). When you integrate SpaceIQ with Azure AD, you can:

* Control in Azure AD who has access to SpaceIQ.
* Enable your users to be automatically signed-in to SpaceIQ with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To configure Azure AD integration with SpaceIQ, you need the following items:

* An Azure AD subscription. If you don't have an Azure AD environment, you can get a [free account](https://azure.microsoft.com/free/).
* SpaceIQ single sign-on enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* SpaceIQ supports **IDP** initiated SSO.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add SpaceIQ from the gallery

To configure the integration of SpaceIQ into Azure AD, you need to add SpaceIQ from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **SpaceIQ** in the search box.
1. Select **SpaceIQ** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for SpaceIQ

Configure and test Azure AD SSO with SpaceIQ using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in SpaceIQ.

To configure and test Azure AD SSO with SpaceIQ, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure SpaceIQ SSO](#configure-spaceiq-sso)** - to configure the single sign-on settings on application side.
    1. **[Create SpaceIQ test user](#create-spaceiq-test-user)** - to have a counterpart of B.Simon in SpaceIQ that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **SpaceIQ** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Set up Single Sign-On with SAML** page, perform the following steps:

    a. In the **Identifier** text box, type the URL:
    `https://api.spaceiq.com`

    b. In the **Reply URL** text box, type a URL using the following pattern:
    `https://api.spaceiq.com/saml/<INSTANCE_ID>/callback`

	> [!NOTE]
	> Update these values with the actual Reply URL and identifier which is explained later in the tutorial.

5. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, click **Download** to download the **Certificate (Base64)** from the given options as per your requirement and save it on your computer.

	![The Certificate download link](common/certificatebase64.png)

6. On the **Set up SpaceIQ** section, copy the appropriate URL(s) as per your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to SpaceIQ.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **SpaceIQ**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure SpaceIQ SSO

1. Open a new browser window, and then sign in to your SpaceIQ environment as an administrator.

1. Once you are logged in, click on the puzzle sign at the top right, then click on **Integrations**

	![Account settings](./media/spaceiq-tutorial/setting.png) 

1. Under **All PROVISIONING & SSO**, click on the **Azure** tile to add an instance of Azure as IDP.

	![SAML icon](./media/spaceiq-tutorial/azure.png)

1. In the **SSO** dialog box, perform the following steps:

    ![SAML Authentication Settings](./media/spaceiq-tutorial/configuration.png)

	a. In the **SAML Issuer URL** box, paste the **Azure AD Identifier** value copied from the Azure AD application configuration window.

	b. Copy the **SAML CallBack Endpoint URL (read-only)** value and paste the value in the **Reply URL** box in the **Basic SAML Configuration** section in the Azure portal.

	c. Copy the **SAML Audience URI (read-only)** value and paste the value in the **Identifier** box in the **Basic SAML Configuration** section in the Azure portal.

	d. Open the downloaded certificate file in notepad, copy the content, and then paste it in the **X.509 Certificate** box.

	e. Click **Save**.

### Create SpaceIQ test user

In this section, you create a user called Britta Simon in SpaceIQ. Work [SpaceIQ support team](mailto:eng@spaceiq.com) to add the users in the SpaceIQ platform. Users must be created and activated before you use single sign-on.

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options.

* Click on Test this application in Azure portal and you should be automatically signed in to the SpaceIQ for which you set up the SSO.

* You can use Microsoft My Apps. When you click the SpaceIQ tile in the My Apps, you should be automatically signed in to the SpaceIQ for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure SpaceIQ you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

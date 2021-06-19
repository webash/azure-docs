---
title: 'Tutorial: Azure Active Directory integration with QuickHelp | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and QuickHelp.
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
# Tutorial: Azure Active Directory integration with QuickHelp

In this tutorial, you'll learn how to integrate QuickHelp with Azure Active Directory (Azure AD). When you integrate QuickHelp with Azure AD, you can:

* Control in Azure AD who has access to QuickHelp.
* Enable your users to be automatically signed-in to QuickHelp with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* QuickHelp single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* QuickHelp supports **SP** initiated SSO.

* QuickHelp supports **Just In Time** user provisioning.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add QuickHelp from the gallery

To configure the integration of QuickHelp into Azure AD, you need to add QuickHelp from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **QuickHelp** in the search box.
1. Select **QuickHelp** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for QuickHelp

Configure and test Azure AD SSO with QuickHelp using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in QuickHelp.

To configure and test Azure AD SSO with QuickHelp, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure QuickHelp SSO](#configure-quickhelp-sso)** - to configure the single sign-on settings on application side.
    1. **[Create QuickHelp test user](#create-quickhelp-test-user)** - to have a counterpart of B.Simon in QuickHelp that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **QuickHelp** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, perform the following steps:

    a. In the **Identifier (Entity ID)** text box, type the URL:
    `https://auth.quickhelp.com`

	b. In the **Sign on URL** text box, type a URL using the following pattern:
    `https://quickhelp.com/<ROUTE_URL>`

	> [!NOTE]
	> The Sign-on URL value is not real. Update the value with the actual Sign-On URL. Contact your organization’s QuickHelp administrator or your BrainStorm Client Success Manager to get the value. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

5. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, click **Download** to download the **Federation Metadata XML** from the given options as per your requirement and save it on your computer.

	![The Certificate download link](common/metadataxml.png)

6. On the **Set up QuickHelp** section, copy the appropriate URL(s) as per your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to QuickHelp.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **QuickHelp**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure QuickHelp SSO

1. Sign in to your QuickHelp company site as administrator.

2. In the menu on the top, click **Admin**.
   
    ![Screenshot shows the Admin menu item for Brainstorm](./media/quickhelp-tutorial/admin.png "Admin menu")

3. In the **QuickHelp Admin** menu, click **Settings**.
   
    ![Screenshot shows Settings selected from the QuickHelp Admin menu](./media/quickhelp-tutorial/menu.png "Settings")

4. Click **Authentication Settings**.

5. On the **Authentication Settings** page, perform the following steps.
   
    ![Screenshot shows the Authentication Settings page where you can enter the values described](./media/quickhelp-tutorial/authenticate.png "Authentication Settings")
   
    a. As **SSO Type**, select **WSFederation**.
   
    b. To upload your downloaded Azure metadata file, click **Browse**, navigate to the file, end then click **Upload Metadata**.
   
    c. In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
   
    d. In the **First Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
   
    e. In the **Last Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
   
    f. In the **Action Bar**, click **Save**.

### Create QuickHelp test user

In this section, a user called Britta Simon is created in QuickHelp. QuickHelp supports just-in-time user provisioning, which is enabled by default. There is no action item for you in this section. If a user doesn't already exist in QuickHelp, a new one is created after authentication.

## Test SSO 

In this section, you test your Azure AD single sign-on configuration with following options. 

* Click on **Test this application** in Azure portal. This will redirect to QuickHelp Sign-on URL where you can initiate the login flow. 

* Go to QuickHelp Sign-on URL directly and initiate the login flow from there.

* You can use Microsoft My Apps. When you click the QuickHelp tile in the My Apps, this will redirect to QuickHelp Sign-on URL. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure QuickHelp you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

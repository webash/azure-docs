---
title: 'Tutorial: Azure Active Directory integration with Panopto | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Panopto.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/28/2021
ms.author: jeedes
---
# Tutorial: Azure Active Directory integration with Panopto

In this tutorial, you'll learn how to integrate Panopto with Azure Active Directory (Azure AD). When you integrate Panopto with Azure AD, you can:

* Control in Azure AD who has access to Panopto.
* Enable your users to be automatically signed-in to Panopto with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Panopto single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* Panopto supports **SP** initiated SSO.

* Panopto supports **Just In Time** user provisioning.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add Panopto from the gallery

To configure the integration of Panopto into Azure AD, you need to add Panopto from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Panopto** in the search box.
1. Select **Panopto** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Panopto

Configure and test Azure AD SSO with Panopto using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Panopto.

To configure and test Azure AD SSO with Panopto, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Panopto SSO](#configure-panopto-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Panopto test user](#create-panopto-test-user)** - to have a counterpart of B.Simon in Panopto that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Panopto** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, perform the following steps:

    In the **Sign-on URL** text box, type a URL using the following pattern:
    `https://<TENANT_NAME>.panopto.com`

	> [!NOTE]
	> The value is not real. Update the value with the actual Sign-On URL. Contact [Panopto Client support team](mailto:support@panopto.com) to get the value. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

5. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, click **Download** to download the **Federation Metadata XML** from the given options as per your requirement and save it on your computer.

	![The Certificate download link](common/metadataxml.png)

6. On the **Set up Panopto** section, copy the appropriate URL(s) as per your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Panopto.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Panopto**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Panopto SSO

1. In a different web browser window, log in to your Panopto company site as an administrator.

2. In the toolbar on the left, click **System**, and then click **Identity Providers**.
   
    ![System](./media/panopto-tutorial/toolbar.png "System")

3. Click **Add Provider**.
   
    ![Identity Providers](./media/panopto-tutorial/provider.png "Identity Providers")
   
4. In the SAML provider section, perform the following steps:
   
    ![SaaS configuration](./media/panopto-tutorial/configuration.png "SaaS configuration")
	
	a. From the **Provider Type** list, select **SAML20**.    
	
	b. In the **Instance Name** textbox, type a name for the instance.

	c. In the **Friendly Description** textbox, type a friendly description.
	
	d. In **Bounce Page Url** textbox, paste the value of **Login URL**, which you have copied from Azure portal.

	e. In the **Issuer** textbox, paste the value of **Azure AD Identifier**, which you have copied from Azure portal.

	f. Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy the content of it in to your clipboard, and then paste it to the **Public Key**  textbox.

5. Click **Save**.

### Create Panopto test user

In this section, a user called Britta Simon is created in Panopto. Panopto supports just-in-time user provisioning, which is enabled by default. There is no action item for you in this section. If a user doesn't already exist in Panopto, a new one is created after authentication.

>[!NOTE]
>You can use any other Panopto user account creation tools or APIs provided by Panopto to provision Azure AD user accounts.
>

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

* Click on **Test this application** in Azure portal. This will redirect to Panopto Sign-on URL where you can initiate the login flow. 

* Go to Panopto Sign-on URL directly and initiate the login flow from there.

* You can use Microsoft My Apps. When you click the Panopto tile in the My Apps, this will redirect to Panopto Sign-on URL. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Panopto you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

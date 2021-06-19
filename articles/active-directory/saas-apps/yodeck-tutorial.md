---
title: 'Tutorial: Azure Active Directory integration with Yodeck | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Yodeck.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/02/2021
ms.author: jeedes
---
# Tutorial: Azure Active Directory integration with Yodeck

In this tutorial, you'll learn how to integrate Yodeck with Azure Active Directory (Azure AD). When you integrate Yodeck with Azure AD, you can:

* Control in Azure AD who has access to Yodeck.
* Enable your users to be automatically signed-in to Yodeck with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To configure Azure AD integration with Yodeck, you need the following items:

* An Azure AD subscription. If you don't have an Azure AD environment, you can get a [free account](https://azure.microsoft.com/free/).
* Yodeck single sign-on enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* Yodeck supports **SP** and **IDP** initiated SSO.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add Yodeck from the gallery

To configure the integration of Yodeck into Azure AD, you need to add Yodeck from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Yodeck** in the search box.
1. Select **Yodeck** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Yodeck

Configure and test Azure AD SSO with Yodeck using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Yodeck.

To configure and test Azure AD SSO with Yodeck, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Yodeck SSO](#configure-yodeck-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Yodeck test user](#create-yodeck-test-user)** - to have a counterpart of B.Simon in Yodeck that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Yodeck** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, if you wish to configure the application in **IDP** initiated mode, perform the following step:

    In the **Identifier** text box, type the URL:
    `https://app.yodeck.com/api/v1/account/metadata/`

5. Click **Set additional URLs** and perform the following step if you wish to configure the application in **SP** initiated mode:

    In the **Sign-on URL** text box, type the URL:
    `https://app.yodeck.com/login`

6. On the **Set up Single Sign-On with SAML** page, In the **SAML Signing Certificate** section, click copy button to copy **App Federation Metadata Url** and save it on your computer.

	![The Certificate download link](common/copy-metadataurl.png)

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Yodeck.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Yodeck**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Yodeck SSO

1. To automate the configuration within **Yodeck**, you need to install **My Apps Secure Sign-in browser extension** by clicking **Install the extension**.

	![Screenshot shows the Install the extension button.](./media/target-process-tutorial/install_extension.png)

1. After adding extension to the browser, click on **setup Yodeck** will direct you to the Yodeck application. From there, provide the admin credentials to sign into Yodeck. The browser extension will automatically configure the application for you and automate steps 3-5.

	![Setup configuration](common/setup-sso.png)

	**If you want to configure the application manually perform the following steps:**

1. In a different web browser window, sign in to your Yodeck company site as an administrator.

1. Click on **User Settings** option form the top right corner of the page and select **Account Settings**.

	![Screenshot shows with Account Settings selected for the user.](./media/yodeck-tutorial/account.png)

1. Select **SAML** and perform the following steps:

	![Screenshot shows the SAML tab where you can perform these steps.](./media/yodeck-tutorial/configure.png)

	a. Select **Import from URL**.

	b. In the **URL** textbox, paste the **App Federation Metadata Url** value, which you have copied from the Azure portal and click **Import**.
	
	c. After importing **App Federation Metadata Url**, the remaining fields populate automatically.

	d. Click **Save**.

### Create Yodeck test user

To enable Azure AD users to sign in to Yodeck, they must be provisioned into Yodeck. In the case of Yodeck, provisioning is a manual task.

**To provision a user account, perform the following steps:**

1. Sign in to your Yodeck company site as an administrator.

2. Click on **User Settings** option form the top right corner of the page and select **Users**.

	![Screenshot shows with Users selected for the user.](./media/yodeck-tutorial/user.png)

3. Click on **+User** to open the **User Details** tab.

	![Screenshot shows the Users button.](./media/yodeck-tutorial/user-details.png)

4. On the **User Details** dialog page, perform the following steps:

	![Screenshot shows the User Details tab where you can perform these steps.](./media/yodeck-tutorial/user-page.png)

	a. In the **First Name** textbox, type the first name of the user like **Britta**.

	b. In the **Last Name** textbox, type the last name of user like **Simon**.

	c. In the **Email** textbox, type the email address of user like brittasimon@contoso.com.

	d. Select appropriate **Account Permissions** option as per your organizational requirement.
	
	e. Click **Save**.

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

#### SP initiated:

* Click on **Test this application** in Azure portal. This will redirect to Yodeck Sign on URL where you can initiate the login flow.  

* Go to Yodeck Sign-on URL directly and initiate the login flow from there.

#### IDP initiated:

* Click on **Test this application** in Azure portal and you should be automatically signed in to the Yodeck for which you set up the SSO. 

You can also use Microsoft My Apps to test the application in any mode. When you click the Yodeck tile in the My Apps, if configured in SP mode you would be redirected to the application sign on page for initiating the login flow and if configured in IDP mode, you should be automatically signed in to the Yodeck for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Yodeck you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

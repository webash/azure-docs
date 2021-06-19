---
title: 'Tutorial: Azure Active Directory integration with XaitPorter | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and XaitPorter.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/31/2021
ms.author: jeedes
---
# Tutorial: Azure Active Directory integration with XaitPorter

In this tutorial, you'll learn how to integrate XaitPorter with Azure Active Directory (Azure AD). When you integrate XaitPorter with Azure AD, you can:

* Control in Azure AD who has access to XaitPorter.
* Enable your users to be automatically signed-in to XaitPorter with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To configure Azure AD integration with XaitPorter, you need the following items:

* An Azure AD subscription. If you don't have an Azure AD environment, you can get a [free account](https://azure.microsoft.com/free/).
* XaitPorter single sign-on enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* XaitPorter supports **SP** initiated SSO.

## Add XaitPorter from the gallery

To configure the integration of XaitPorter into Azure AD, you need to add XaitPorter from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **XaitPorter** in the search box.
1. Select **XaitPorter** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for XaitPorter

Configure and test Azure AD SSO with XaitPorter using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in XaitPorter.

To configure and test Azure AD SSO with XaitPorter, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure XaitPorter SSO](#configure-xaitporter-sso)** - to configure the single sign-on settings on application side.
    1. **[Create XaitPorter test user](#create-xaitporter-test-user)** - to have a counterpart of B.Simon in XaitPorter that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **XaitPorter** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, perform the following steps:

	a. In the **Identifier (Entity ID)** text box, type a URL using the following pattern:
    `https://<SUBDOMAIN>.xaitporter.com`

	b. In the **Sign on URL** text box, type a URL using the following pattern:
    `https://<SUBDOMAIN>.xaitporter.com/saml/login`

	> [!NOTE]
	> These values are not real. Update these values with the actual identifier and Sign on URL. Contact [XaitPorter Client support team](https://www.xait.com/support/) to get these values. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

5. On the **Set up Single Sign-On with SAML** page, In the **SAML Signing Certificate** section, click copy button to copy **App Federation Metadata Url** and save it on your computer.

	![The Certificate download link](common/copy-metadataurl.png)

6. Provide the **IP address** or the **App Federation Metadata Url** to the [SmartRecruiters support team](https://www.smartrecruiters.com/about-us/contact-us/), so that XaitPorter can ensure that IP address is reachable from your XaitPorter instance configuring approved list at their side. 

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to XaitPorter.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **XaitPorter**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure XaitPorter SSO

1. To automate the configuration within XaitPorter, you need to install **My Apps Secure Sign-in browser extension** by clicking **Install the extension**.

	![My apps extension](common/install-myappssecure-extension.png)

2. After adding extension to the browser, click on **Setup XaitPorter** will direct you to the XaitPorter application. From there, provide the admin credentials to sign into XaitPorter. The browser extension will automatically configure the application for you and automate steps 3-6.

	![Setup configuration](common/setup-sso.png)

3. If you want to setup XaitPorter manually, open a new web browser window and sign into your XaitPorter company site as an administrator and perform the following steps:

4. Click on **Admin**.

	![Screenshot shows Admin selected in the XaitPorter site.](./media/xaitporter-tutorial/admin.png)

5. Select **Manage Single Sign-On** from the **System Setup** dropdown list.

	![Screenshot shows Manage Single Sign-On selected from System Setup.](./media/xaitporter-tutorial/user.png)

6. In the **MANAGE SINGLE SIGN-ON** section, perform the following steps:

	![Screenshot shows the MANAGE SINGLE SIGN-ON section where you can perform these steps.](./media/xaitporter-tutorial/authentication.png)

	a. Select **Enable Single Sign-On Authentication**.

	b. In **Identity Provider Settings** textbox, paste **App Federation Metadata Url** which you have copied from the Azure portal and click **Fetch**.

	c. Select **Enable Autocreation of Users**.

	d. Click **OK**.

### Create XaitPorter test user

In this section, you create a user called Britta Simon in XaitPorter. Work with [XaitPorter Client support team](https://www.xait.com/support/) to add the users in the XaitPorter platform. Users must be created and activated before you use single sign-on.

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

* Click on **Test this application** in Azure portal. This will redirect to XaitPorter Sign-on URL where you can initiate the login flow. 

* Go to XaitPorter Sign-on URL directly and initiate the login flow from there.

* You can use Microsoft My Apps. When you click the XaitPorter tile in the My Apps, this will redirect to XaitPorter Sign-on URL. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure XaitPorter you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

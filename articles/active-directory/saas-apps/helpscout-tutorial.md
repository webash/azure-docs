---
title: 'Tutorial: Azure Active Directory integration with Help Scout | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Help Scout.
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
# Tutorial: Azure Active Directory integration with Help Scout

In this tutorial, you'll learn how to integrate Help Scout with Azure Active Directory (Azure AD). When you integrate Help Scout with Azure AD, you can:

* Control in Azure AD who has access to Help Scout.
* Enable your users to be automatically signed-in to Help Scout with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To configure Azure AD integration with Help Scout, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Help Scout single sign-on enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* Help Scout supports **SP and IDP** initiated SSO.
* Help Scout supports **Just In Time** user provisioning.

## Add Help Scout from the gallery

To configure the integration of Help Scout into Azure AD, you need to add Help Scout from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Help Scout** in the search box.
1. Select **Help Scout** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Help Scout

Configure and test Azure AD SSO with Help Scout using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Help Scout.

To configure and test Azure AD SSO with Help Scout, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Help Scout SSO](#configure-help-scout-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Help Scout test user](#create-help-scout-test-user)** - to have a counterpart of B.Simon in Help Scout that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Help Scout** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

1. On the **Basic SAML Configuration** section, If you wish to configure the application in **IDP** initiated mode, perform the following steps:

    a. **Identifier** is the **Audience URI (Service Provider Entity ID)** from Help Scout, starts with `urn:`

	b. **Reply URL** is the **Post-back URL (Assertion Consumer Service URL)** from Help Scout, starts with `https://` 

	> [!NOTE]
	> The values in these URLs are for demonstration only. You need to update these values from actual Reply URL and Identifier. You get these values from the **Single Sign-On** tab under Authentication section, which is explained later in the tutorial.

1. Click **Set additional URLs** and perform the following step if you wish to configure the application in **SP** initiated mode:

    In the **Sign-on URL** textbox, type the URL: `https://secure.helpscout.net/members/login/`

1. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, click **Download** to download the **Certificate (Base64)** from the given options as per your requirement and save it on your computer.

	![The Certificate download link](common/certificatebase64.png)

1. On the **Set up Help Scout** section, copy the appropriate URL(s) as per your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Help Scout.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Help Scout**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Help Scout SSO

1. To automate the configuration within Help Scout, you need to install **My Apps Secure Sign-in browser extension** by clicking **Install the extension**.

	![My apps extension](common/install-myappssecure-extension.png)

1. After adding extension to the browser, click on **Set up Help Scout** will direct you to the Help Scout application. From there, provide the admin credentials to sign into Help Scout. The browser extension will automatically configure the application for you and automate steps 3-7.

	![Setup configuration](common/setup-sso.png)

1. If you want to setup Help Scout manually, open a new web browser window and sign into your Help Scout company site as an administrator and perform the following steps:

1. Click on **Manage** from the top menu and then select **Company** from the dropdown menu.

	![Screenshot shows the Manage menu with Company selected.](./media/helpscout-tutorial/settings.png)

1. Select **Authentication** from the left navigation pane.

	![Screenshot shows Authentication selected.](./media/helpscout-tutorial/authentication.png)

1. This takes you to the SAML settings section and perform the following steps:

	![Screenshot shows the Single Sign-On tab where you enter the specified information.](./media/helpscout-tutorial/configuration.png)

	a. Copy the **Post-back URL (Assertion Consumer Service URL)** value and paste the value in the **Reply URL** text box in the **Basic SAML Configuration** section in the Azure portal.

	b. Copy the **Audience URI (Service Provider Entity ID)** value and paste the value in the **Identifier** text box in the **Basic SAML Configuration** section in the Azure portal.

1. Toggle **Enable SAML** on and perform the following steps:

	![Screenshot shows the Single Sign-On tab where you enable SAML and add other information.](./media/helpscout-tutorial/information.png)

	a. In **Single Sign-On URL** textbox, paste the value of **Login URL**, which you have copied from Azure portal.

	b. Click **Upload Certificate** to upload the **Certificate(Base64)** downloaded from Azure portal.

	c. Enter your organization's email domain(s) e.x.- `contoso.com` in the **Email Domains** textbox. You can separate multiple domains with a comma. Anytime a Help Scout User or Administrator who enters that specific domain on the [Help Scout log-in page](https://secure.helpscout.net/members/login/) will be routed to Identity Provider to authenticate with their credentials.

	d. Lastly, you can toggle **Force SAML Sign-on** if you want Users to only log in to Help Scout via through this method. If you'd still like to leave the option for them to sign in with their Help Scout credentials, you can leave it toggled off. Even if this is enabled, an Account Owner will always be able to log in to Help Scout with their account password.

	e. Click **Save**.

### Create Help Scout test user

In this section, a user called B.Simon is created in Help Scout. Help Scout supports just-in-time user provisioning, which is enabled by default. There is no action item for you in this section. If a user doesn't already exist in Help Scout, a new one is created after authentication.

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

#### SP initiated:

* Click on **Test this application** in Azure portal. This will redirect to Help Scout Sign on URL where you can initiate the login flow.  

* Go to Help Scout Sign-on URL directly and initiate the login flow from there.

#### IDP initiated:

* Click on **Test this application** in Azure portal and you should be automatically signed in to the Help Scout for which you set up the SSO. 

You can also use Microsoft My Apps to test the application in any mode. When you click the Help Scout tile in the My Apps, if configured in SP mode you would be redirected to the application sign on page for initiating the login flow and if configured in IDP mode, you should be automatically signed in to the Help Scout for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Help Scout you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

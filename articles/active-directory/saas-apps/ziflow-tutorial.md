---
title: 'Tutorial: Azure Active Directory integration with Ziflow | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Ziflow.
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
# Tutorial: Azure Active Directory integration with Ziflow

In this tutorial, you'll learn how to integrate Ziflow with Azure Active Directory (Azure AD). When you integrate Ziflow with Azure AD, you can:

* Control in Azure AD who has access to Ziflow.
* Enable your users to be automatically signed-in to Ziflow with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To configure Azure AD integration with Ziflow, you need the following items:

* An Azure AD subscription. If you don't have an Azure AD environment, you can get a [free account](https://azure.microsoft.com/free/).
* Ziflow single sign-on enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* Ziflow supports **SP** initiated SSO.

## Add Ziflow from the gallery

To configure the integration of Ziflow into Azure AD, you need to add Ziflow from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Ziflow** in the search box.
1. Select **Ziflow** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Ziflow

Configure and test Azure AD SSO with Ziflow using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Ziflow.

To configure and test Azure AD SSO with Ziflow, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Ziflow SSO](#configure-ziflow-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Ziflow test user](#create-ziflow-test-user)** - to have a counterpart of B.Simon in Ziflow that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Ziflow** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, perform the following steps:

	a. In the **Identifier (Entity ID)** text box, type a value using the following pattern:
    `urn:auth0:ziflow-production:<UNIQUE_ID>`

	b. In the **Sign on URL** text box, type a URL using the following pattern:
    `https://ziflow-production.auth0.com/login/callback?connection=<UNIQUE_ID>`

	> [!NOTE]
	> The preceding values are not real. You will update the unique ID value in the Identifier and Sign on URL with actual value, which is explained later in the tutorial.

5. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, click **Download** to download the **Certificate (Base64)** from the given options as per your requirement and save it on your computer.

	![The Certificate download link](common/certificatebase64.png)

6. On the **Set up Ziflow** section, copy the appropriate URL(s) as per your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Ziflow.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Ziflow**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Ziflow SSO

1. In a different web browser window, sign in to Ziflow as a Security Administrator.

2. Click on Avatar in the top right corner, and then click **Manage account**.

	![Ziflow Configuration Manage](./media/ziflow-tutorial/manage-account.png)

3. In the top left, click **Single Sign-On**.

	![Ziflow Configuration Sign](./media/ziflow-tutorial/configuration.png)

4. On the **Single Sign-On** page, perform the following steps:

	![Ziflow Configuration Single](./media/ziflow-tutorial/page.png)

	a. Select **Type** as **SAML2.0**.

	b. In the **Sign In URL** textbox, paste the value of **Login URL**, which you have copied from the Azure portal.

    c. Upload the base-64 encoded certificate that you have downloaded from the Azure portal, into the **X509 Signing Certificate**.

	d. In the **Sign Out URL** textbox, paste the value of **Logout URL**, which you have copied from the Azure portal.

	e. From the **Configuration Settings for your Identifier Provider** section, copy the highlighted unique ID value and append it with the Identifier and Sign on URL in the **Basic SAML Configuration** on Azure portal.

### Create Ziflow test user

To enable Azure AD users to sign in to Ziflow, they must be provisioned into Ziflow. In Ziflow, provisioning is a manual task.

To provision a user account, perform the following steps:

1. Sign in to Ziflow as a Security Administrator.

2. Navigate to **People** on the top.

	![Ziflow Configuration people](./media/ziflow-tutorial/people.png)

3. Click **Add** and then click **Add user**.

	![Screenshot shows the Add user option selected.](./media/ziflow-tutorial/add-tab.png)

4. On the **Add a user** popup, perform the following steps:

	![Screenshot shows the Add a user dialog box where you can enter the values described.](./media/ziflow-tutorial/add-user.png)

	a. In **Email** text box, enter the email of user like brittasimon@contoso.com.

	b. In **First name** text box, enter the first name of user like Britta.

	c. In **Last name** text box, enter the last name of user like Simon.

	d. Select your Ziflow role.

	e. Click **Add 1 user**.

	> [!NOTE]
    > The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.

## Test SSO 

In this section, you test your Azure AD single sign-on configuration with following options. 

* Click on **Test this application** in Azure portal. This will redirect to Ziflow Sign-on URL where you can initiate the login flow. 

* Go to Ziflow Sign-on URL directly and initiate the login flow from there.

* You can use Microsoft My Apps. When you click the Ziflow tile in the My Apps, this will redirect to Ziflow Sign-on URL. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Ziflow you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

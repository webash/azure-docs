---
title: 'Tutorial: Azure Active Directory integration with LearnUpon | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and LearnUpon.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/03/2021
ms.author: jeedes
---
# Tutorial: Azure Active Directory integration with LearnUpon

In this tutorial, you'll learn how to integrate LearnUpon with Azure Active Directory (Azure AD). When you integrate LearnUpon with Azure AD, you can:

* Control in Azure AD who has access to LearnUpon.
* Enable your users to be automatically signed-in to LearnUpon with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To configure Azure AD integration with LearnUpon, you need the following items:

* An Azure AD subscription. If you don't have an Azure AD environment, you can get a [free account](https://azure.microsoft.com/free/).
* LearnUpon single sign-on enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* LearnUpon supports **IDP** initiated SSO.

* LearnUpon supports **Just In Time** user provisioning.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add LearnUpon from the gallery

To configure the integration of LearnUpon into Azure AD, you need to add LearnUpon from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **LearnUpon** in the search box.
1. Select **LearnUpon** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for LearnUpon

Configure and test Azure AD SSO with LearnUpon using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in LearnUpon.

To configure and test Azure AD SSO with LearnUpon, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure LearnUpon SSO](#configure-learnupon-sso)** - to configure the single sign-on settings on application side.
    1. **[Create LearnUpon test user](#create-learnupon-test-user)** - to have a counterpart of B.Simon in LearnUpon that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **LearnUpon** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, perform the following steps:

    In the **Reply URL** text box, type a URL using the following pattern:
    `https://<companyname>.learnupon.com/saml/consumer`

	> [!NOTE]
	> The value is not real. Update the value with the actual Reply URL. Contact [LearnUpon Client support team](https://www.learnupon.com/features/support/) to get the value. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

5. On the **Set up Single Sign-On with SAML** page, locate the **THUMBPRINT** - This will be added to your LearnUpon SAML Settings.

	![The Certificate download link](common/certificateraw.png)

6. On the **Set up LearnUpon** section, copy the appropriate URL(s) as per your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to LearnUpon.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **LearnUpon**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure LearnUpon SSO

1. Open another browser instance and sign in into LearnUpon with an administrator account.

1. Click the **settings** tab.

    ![Screenshot shows the settings tab.](./media/learnupon-tutorial/settings.png)

1. Click **Single Sign On - SAML**, and then click **General Settings** to configure SAML settings.
   
    ![Screenshot shows Single Sign On - SAML selected with General Settings selected.](./media/learnupon-tutorial/general-settings.png) 

1. In the **General Settings** section, perform the following steps:
   
    ![Screenshot shows the General Settings section where you can enter the values described.](./media/learnupon-tutorial/values.png)  
  
	a. Select **Enabled**.

	b. Select **Version** as **2.0**.

	c. Select **Skip conditions** as **No**.

	d. In the **SAML Token Post param name** textbox, type the name of request post parameter to the SAML consumer URL indicated above that contains the SAML Assertion to be verified and authenticated - for example **SAMLResponse**.

	e. In the **Name Identifier Format** textbox, type the value that indicates where in your SAML Assertion the users identifier (Email address) resides - for example `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.
  
	f. In the **Identify Provider Location** textbox, type the value that indicates where the users are sent to if they click on your uploaded icon from your Azure portal login screen.
  
	g. In the **Sign out URL** textbox, paste the **Logout URL** value, which you have copied from the Azure portal.

	h. Click **Manage finger prints**, and then upload the finger print of your downloaded certificate.

1. Click **User Settings**, and then perform the following steps:

     ![Screenshot shows the User Settings section where you can enter the values described.](./media/learnupon-tutorial/user-settings.png)  

	a. In the **First Name Identifier Format** textbox, type the value that tells us where in your SAML Assertion the users firstname resides - for example: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
  
	b. In the **Last Name Identifier Format** textbox, type the value that tells us where in your SAML Assertion the users lastname resides - for example: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

### Create LearnUpon test user

In this section, a user called Britta Simon is created in LearnUpon. LearnUpon supports just-in-time user provisioning, which is enabled by default. There is no action item for you in this section. If a user doesn't already exist in LearnUpon, a new one is created after authentication. If you need to create an user manually, you need to contact [LearnUpon support team](https://www.learnupon.com/features/support/).

## Test SSO 

In this section, you test your Azure AD single sign-on configuration with following options.

* Click on Test this application in Azure portal and you should be automatically signed in to the LearnUpon for which you set up the SSO.

* You can use Microsoft My Apps. When you click the LearnUpon tile in the My Apps, you should be automatically signed in to the LearnUpon for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure LearnUpon you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

---
title: 'Tutorial: Azure Active Directory integration with Hosted Graphite | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Hosted Graphite.
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
# Tutorial: Azure Active Directory integration with Hosted Graphite

In this tutorial, you'll learn how to integrate Hosted Graphite with Azure Active Directory (Azure AD). When you integrate Hosted Graphite with Azure AD, you can:

* Control in Azure AD who has access to Hosted Graphite.
* Enable your users to be automatically signed-in to Hosted Graphite with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Hosted Graphite single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* Hosted Graphite supports **SP and IDP** initiated SSO.
* Hosted Graphite supports **Just In Time** user provisioning.

## Add Hosted Graphite from the gallery

To configure the integration of Hosted Graphite into Azure AD, you need to add Hosted Graphite from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Hosted Graphite** in the search box.
1. Select **Hosted Graphite** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Hosted Graphite

Configure and test Azure AD SSO with Hosted Graphite using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Hosted Graphite.

To configure and test Azure AD SSO with Hosted Graphite, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Hosted Graphite SSO](#configure-hosted-graphite-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Hosted Graphite test user](#create-hosted-graphite-test-user)** - to have a counterpart of B.Simon in Hosted Graphite that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Hosted Graphite** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, If you wish to configure the application in **IDP** initiated mode, perform the following steps:

    a. In the **Identifier** text box, type a URL using the following pattern:
    `https://www.hostedgraphite.com/metadata/<USER_ID>`

    b. In the **Reply URL** text box, type a URL using the following pattern:
    `https://www.hostedgraphite.com/complete/saml/<USER_ID>`

5. Click **Set additional URLs** and perform the following step if you wish to configure the application in **SP** initiated mode:

    In the **Sign-on URL** text box, type a URL using the following pattern:
    `https://www.hostedgraphite.com/login/saml/<USER_ID>/`

    > [!NOTE]
    > Please note that these are not the real values. You have to update these values with the actual Identifier, Reply URL and Sign On URL. To get these values, you can go to Access->SAML setup on your Application side or Contact [Hosted Graphite support team](mailto:help@hostedgraphite.com).

6. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, click **Download** to download the **Certificate (Base64)** from the given options as per your requirement and save it on your computer.

	![The Certificate download link](common/certificatebase64.png)

7. On the **Set up Hosted Graphite** section, copy the appropriate URL(s) as per your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Hosted Graphite.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Hosted Graphite**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Hosted Graphite SSO

1. Sign-on to your Hosted Graphite tenant as an administrator.

2. Go to the **SAML Setup page** in the sidebar (**Access -> SAML Setup**).

    ![Screenshot shows the Access menu with SAML Setup selected.](./media/hostedgraphite-tutorial/setup.png)

3. Confirm these URLs match your configuration done on the **Basic SAML Configuration** section of the Azure portal.

    ![Screenshot shows Basic SAML Configuration.](./media/hostedgraphite-tutorial/configuration.png)

4. In  **Entity or Issuer ID** and **SSO Login URL** textboxes, paste the value of **Azure Ad Identifier** and **Login URL** which you have copied from Azure portal.

    ![Screenshot shows entries for Identity Provider.](./media/hostedgraphite-tutorial/integration.png)

5. Select **Read-only** as **Default User Role**.

    ![Screenshot shows Default User Role, which is Read-only.](./media/hostedgraphite-tutorial/role.png)

6. Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.

    ![Screenshot shows X dot 509 Certificate.](./media/hostedgraphite-tutorial/certificate.png)

7. Click **Save** button.

### Create Hosted Graphite test user

In this section, a user called Britta Simon is created in Hosted Graphite. Hosted Graphite supports just-in-time user provisioning, which is enabled by default. There is no action item for you in this section. If a user doesn't already exist in Hosted Graphite, a new one is created after authentication.

> [!NOTE]
> If you need to create a user manually, you need to contact the [Hosted Graphite support team](<mailto:help@hostedgraphite.com>).

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

#### SP initiated:

* Click on **Test this application** in Azure portal. This will redirect to Hosted Graphite Sign on URL where you can initiate the login flow.  

* Go to Hosted Graphite Sign-on URL directly and initiate the login flow from there.

#### IDP initiated:

* Click on **Test this application** in Azure portal and you should be automatically signed in to the Hosted Graphite for which you set up the SSO. 

You can also use Microsoft My Apps to test the application in any mode. When you click the Hosted Graphite tile in the My Apps, if configured in SP mode you would be redirected to the application sign on page for initiating the login flow and if configured in IDP mode, you should be automatically signed in to the Hosted Graphite for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Hosted Graphite you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

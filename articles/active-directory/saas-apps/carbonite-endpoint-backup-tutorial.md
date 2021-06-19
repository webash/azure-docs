---
title: 'Tutorial: Azure Active Directory integration with Carbonite Endpoint Backup | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Carbonite Endpoint Backup.
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

# Tutorial: Integrate Carbonite Endpoint Backup with Azure Active Directory

In this tutorial, you'll learn how to integrate Carbonite Endpoint Backup with Azure Active Directory (Azure AD). When you integrate Carbonite Endpoint Backup with Azure AD, you can:

* Control in Azure AD who has access to Carbonite Endpoint Backup.
* Enable your users to be automatically signed-in to Carbonite Endpoint Backup with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Carbonite Endpoint Backup single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD SSO in a test environment.

* Carbonite Endpoint Backup supports **SP and IDP** initiated SSO.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add Carbonite Endpoint Backup from the gallery

To configure the integration of Carbonite Endpoint Backup into Azure AD, you need to add Carbonite Endpoint Backup from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Carbonite Endpoint Backup** in the search box.
1. Select **Carbonite Endpoint Backup** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Carbonite Endpoint Backup

Configure and test Azure AD SSO with Carbonite Endpoint Backup using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Carbonite Endpoint Backup.

To configure and test Azure AD SSO with Carbonite Endpoint Backup, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Carbonite Endpoint Backup SSO](#configure-carbonite-endpoint-backup-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Carbonite Endpoint Backup test user](#create-carbonite-endpoint-backup-test-user)** - to have a counterpart of B.Simon in Carbonite Endpoint Backup that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Carbonite Endpoint Backup** application integration page, find the **Manage** section and select **Single sign-on**.
1. On the **Select a Single sign-on method** page, select **SAML**.
1. On the **Set up Single Sign-On with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

1. On the **Basic SAML Configuration** section, if you wish to configure the application in **IDP** initiated mode, enter the values for the following fields:

    a. In the **Identifier** text box, type one of the following URLs:

    ```http
    https://red-us.mysecuredatavault.com
    https://red-apac.mysecuredatavault.com
    https://red-fr.mysecuredatavault.com
    https://red-emea.mysecuredatavault.com
    https://kamino.mysecuredatavault.com
    ```

    b. In the **Reply URL** text box, type one of the following URLs:

    ```http
    https://red-us.mysecuredatavault.com/AssertionConsumerService.aspx
    https://red-apac.mysecuredatavault.com/AssertionConsumerService.aspx
    https://red-fr.mysecuredatavault.com/AssertionConsumerService.aspx
    https://red-emea.mysecuredatavault.com/AssertionConsumerService.aspx
    ```

1. Click **Set additional URLs** and perform the following step if you wish to configure the application in **SP** initiated mode:

    In the **Sign-on URL** text box, type one of the following URLs:

    ```http
    https://red-us.mysecuredatavault.com/
    https://red-apac.mysecuredatavault.com/
    https://red-fr.mysecuredatavault.com/
    https://red-emea.mysecuredatavault.com/
    ```

1. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section,  find **Certificate (Base64)** and select **Download** to download the certificate and save it on your computer.

	![The Certificate download link](common/certificatebase64.png)

1. On the **Set up Carbonite Endpoint Backup** section, copy the appropriate URL(s) based on your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Carbonite Endpoint Backup.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Carbonite Endpoint Backup**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Carbonite Endpoint Backup SSO

1. To automate the configuration within Carbonite Endpoint Backup, you need to install **My Apps Secure Sign-in browser extension** by clicking **Install the extension**.

	![My apps extension](common/install-myappssecure-extension.png)

2. After adding extension to the browser, click on **Setup Carbonite Endpoint Backup** will direct you to the Carbonite Endpoint Backup application. From there, provide the admin credentials to sign into Carbonite Endpoint Backup. The browser extension will automatically configure the application for you and automate steps 3-7.

	![Setup configuration](common/setup-sso.png)

3. If you want to setup Carbonite Endpoint Backup manually, open a new web browser window and sign into your Carbonite Endpoint Backup company site as an administrator and perform the following steps:

4. Click on the **Company** from the left pane.

    ![Screenshot shows Carbonite Endpoint with Company selected.](media/carbonite-endpoint-backup-tutorial/company.png)

5. Click on **Single sign-on**.

    ![Screenshot shows Company with Single sign-on selected.](media/carbonite-endpoint-backup-tutorial/single-sign-on.png)

6. Click on **Enable** and then click **Edit settings** to configure.

    ![Screenshot shows the Single sign-on tab with Enable and Edit settings called out.](media/carbonite-endpoint-backup-tutorial/settings.png)

7. On the **Single sign-on** settings page, perform the following steps:

    ![Screenshot shows the Single sign-on tab with the information described in this step.](media/carbonite-endpoint-backup-tutorial/save.png)

    1. In the **Identity provider name** textbox, paste the **Azure AD Identifier** value, which you have copied from the Azure portal.

    1. In the **Identity provider URL** textbox, paste the **Login URL** value, which you have copied from the Azure portal.

    1. Click on **Choose file** to upload the downloaded **Certificate(Base64)** file from the Azure portal.

    1. Click **Save**.

### Create Carbonite Endpoint Backup test user

1. In a different web browser window, sign in to your Carbonite Endpoint Backup company site as an administrator.

1. Click on the **Users** from the left pane and then click **Add user**.

    ![Screenshot shows the Carbonite Endpoint page with Users and Add users selected.](media/carbonite-endpoint-backup-tutorial/add-user-1.png)

1. On the **Add user** page, perform the following steps:

    ![Screenshot shows the Add user page where you can perform the steps described here.](media/carbonite-endpoint-backup-tutorial/add-user-2.png)

    1. Enter the **Email**, **First name**, **Last name** of the user and provide the required permissions to the user according to the Organizational requirements.

    1. Click **Add user**.

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

#### SP initiated:

* Click on **Test this application** in Azure portal. This will redirect to Carbonite Endpoint Backup Sign on URL where you can initiate the login flow.  

* Go to Carbonite Endpoint Backup Sign-on URL directly and initiate the login flow from there.

#### IDP initiated:

* Click on **Test this application** in Azure portal and you should be automatically signed in to the Carbonite Endpoint Backup for which you set up the SSO. 

You can also use Microsoft My Apps to test the application in any mode. When you click the Carbonite Endpoint Backup tile in the My Apps, if configured in SP mode you would be redirected to the application sign on page for initiating the login flow and if configured in IDP mode, you should be automatically signed in to the Carbonite Endpoint Backup for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Carbonite Endpoint Backup you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).
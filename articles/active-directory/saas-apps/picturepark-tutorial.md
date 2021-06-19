---
title: 'Tutorial: Azure Active Directory integration with Picturepark | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Picturepark.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/01/2021
ms.author: jeedes
---
# Tutorial: Azure Active Directory integration with Picturepark

In this tutorial, you'll learn how to integrate Picturepark with Azure Active Directory (Azure AD). When you integrate Picturepark with Azure AD, you can:

* Control in Azure AD who has access to Picturepark.
* Enable your users to be automatically signed-in to Picturepark with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To configure Azure AD integration with Picturepark, you need the following items:

* An Azure AD subscription. If you don't have an Azure AD environment, you can get a [free account](https://azure.microsoft.com/free/).
* Picturepark single sign-on enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* Picturepark supports **SP** initiated SSO.

## Add Picturepark from the gallery

To configure the integration of Picturepark into Azure AD, you need to add Picturepark from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Picturepark** in the search box.
1. Select **Picturepark** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Picturepark

Configure and test Azure AD SSO with Picturepark using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Picturepark.

To configure and test Azure AD SSO with Picturepark, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Picturepark SSO](#configure-picturepark-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Picturepark test user](#create-picturepark-test-user)** - to have a counterpart of B.Simon in Picturepark that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Picturepark** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, perform the following steps:

    a. In the **Identifier (Entity ID)** text box, type a URL using one of the following patterns:

    | Identifier URL |
    |---|
    |`https://<COMPANY_NAME>.current-picturepark.com`|
    |`https://<COMPANY_NAME>.picturepark.com`|
    |`https://<COMPANY_NAME>.next-picturepark.com`|
    |
    
    b. In the **Sign on URL** text box, type a URL using the following pattern: 
    `https://<COMPANY_NAME>.picturepark.com`

	> [!NOTE]
	> These values are not real. Update these values with the actual Identifier and Sign on URL. Contact [Picturepark Client support team](https://picturepark.com/company/picturepark-customer-support) to get these values. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

5. In the **SAML Signing Certificate** section, click **Edit** button to open **SAML Signing Certificate** dialog.

	![Edit SAML Signing Certificate](common/edit-certificate.png)

6. In the **SAML Signing Certificate** section, copy the **Thumbprint** and save it on your computer.

    ![Copy Thumbprint value](common/copy-thumbprint.png)

7. On the **Set up Picturepark** section, copy the appropriate URL(s) as per your requirement. For **Login URL**, use the value with the following pattern: `https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ is the tenant id of Azure AD subscription.

	![Copy configuration URLs](./media/picturepark-tutorial/configure.png)

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Picturepark.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Picturepark**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Picturepark SSO

1. In a different web browser window, sign into your Picturepark company site as an administrator.

2. In the toolbar on the top, click **Administrative tools**, and then click **Management Console**.
   
    ![Management Console](./media/picturepark-tutorial/tools.png "Management Console")

3. Click **Authentication**, and then click **Identity providers**.
   
    ![Authentication](./media/picturepark-tutorial/identity-provider.png "Authentication")

4. In the **Identity provider configuration** section, perform the following steps:
   
    ![Identity provider configuration](./media/picturepark-tutorial/add-configuration.png "Identity provider configuration")
   
    a. Click **Add**.
  
    b. Type a name for your configuration.
   
    c. Select **Set as default**.
   
    d. In **Issuer URI** textbox, paste the value of **Login URL** which you have copied from Azure portal.
   
    e. In **Trusted Issuer Thumb Print** textbox, paste the value of **Thumbprint** which you have copied from **SAML Signing Certificate** section. 

5. Click **JoinDefaultUsersGroup**.

6. To set the **Emailaddress** attribute in the **Claim** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` and click **Save**.

      ![Configuration](./media/picturepark-tutorial/claim.png "Configuration")

### Create Picturepark test user

In order to enable Azure AD users to sign into Picturepark, they must be provisioned into Picturepark. In the case of Picturepark, provisioning is a manual task.

**To provision a user account, perform the following steps:**

1. Sign in to your **Picturepark** tenant.

1. In the toolbar on the top, click **Administrative tools**, and then click **Users**.
   
    ![Users](./media/picturepark-tutorial/user.png "Users")

1. In the **Users overview** tab, click **New**.
   
    ![User management](./media/picturepark-tutorial/new-user.png "User management")

1. On the **Create User** dialog, perform the following steps of a valid Azure Active Directory User you want to provision:
   
    ![Create User](./media/picturepark-tutorial/details.png "Create User")
   
    a. In the **Email Address** textbox, type the **email address** of the user `BrittaSimon@contoso.com`.  
   
    b. In the **Password** and **Confirm Password** textboxes, type the **password** of BrittaSimon. 
   
    c. In the **First Name** textbox, type the **First Name** of the user **Britta**. 
   
    d. In the **Last Name** textbox, type the **Last Name** of the user **Simon**.
   
    e. In the **Company** textbox, type the **Company name** of the user. 
   
    f. In the **Country** textbox, select the **Country/Region** of the user.
  
    g. In the **ZIP** textbox, type the **ZIP code** of the city.
   
    h. In the **City** textbox, type the **City name** of the user.

    i. Select a **Language**.
   
    j. Click **Create**.

>[!NOTE]
>You can use any other Picturepark user account creation tools or APIs provided by Picturepark to provision Azure AD user accounts.
>

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

* Click on **Test this application** in Azure portal. This will redirect to Picturepark Sign-on URL where you can initiate the login flow. 

* Go to Picturepark Sign-on URL directly and initiate the login flow from there.

* You can use Microsoft My Apps. When you click the Picturepark tile in the My Apps, this will redirect to Picturepark Sign-on URL. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Picturepark you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

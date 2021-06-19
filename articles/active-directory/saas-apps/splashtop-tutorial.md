---
title: 'Tutorial: Azure Active Directory single sign-on (SSO) integration with Splashtop | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Splashtop.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/19/2021
ms.author: jeedes
---

# Tutorial: Azure Active Directory single sign-on (SSO) integration with Splashtop

In this tutorial, you'll learn how to integrate Splashtop with Azure Active Directory (Azure AD). When you integrate Splashtop with Azure AD, you can:

* Control in Azure AD who has access to Splashtop.
* Enable your users to be automatically signed-in to Splashtop with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Splashtop single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD SSO in a test environment.

* Splashtop supports **SP** initiated SSO.

> [!NOTE]
> Identifier of this application is a fixed string value so only one instance can be configured in one tenant.

## Add Splashtop from the gallery

To configure the integration of Splashtop into Azure AD, you need to add Splashtop from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Splashtop** in the search box.
1. Select **Splashtop** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Splashtop

Configure and test Azure AD SSO with Splashtop using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Splashtop.

To configure and test Azure AD SSO with Splashtop, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Splashtop SSO](#configure-splashtop-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Splashtop test user](#create-splashtop-test-user)** - to have a counterpart of B.Simon in Splashtop that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Splashtop** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set-up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

1. On the **Basic SAML Configuration** section, perform the following step:

    In the **Sign-on URL** text box, type the URL:
    `https://my.splashtop.com/login/sso`

1. Splashtop application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration. The following screenshot shows the list of default attributes, whereas **nameidentifier** is mapped with **user.userprincipalname**. TicketManager application expects **nameidentifier** to be mapped with **user.mail**, so you need to edit the attribute mapping by clicking on **Edit** icon and change the attribute mapping.

	![Screenshot shows User Attributes with the Edit icon selected.](common/edit-attribute.png)

1. On the **Set-up single sign-on with SAML** page, in the **SAML Signing Certificate** section,  find **Certificate (Base64)** and select **Download** to download the certificate and save it on your computer.

	![The Certificate download link](common/certificatebase64.png)

1. On the **Set-up Splashtop** section, copy the appropriate URL(s) based on your requirement.

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

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Splashtop.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Splashtop**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Splashtop SSO

In this section, you will need to apply for a new SSO method from [Splashtop web portal](https://my.splashtop.com/login).
1. In the Splashtop web portal, go to **Account info** / **Team** tab, scroll down to find **Single Sign On** section. Then click **Apply for new SSO method**.

	![Screenshot shows the Single Sign On page where you can select Apply for new S S O method.](media/splashtop-tutorial/new-method.png)

1. On the applying window, give an **SSO name**. For example, New Azure, then select **Azure** as the IDP type, and insert **Login URL** and **Azure AD Identifier** copied from Splashtop application on Azure portal.

	![Screenshot shows the Apply for S S O method page where you can enter a name and other information.](media/splashtop-tutorial/new-azure.png)

1. For certificate info, right-click on the cert file downloaded from Splashtop application on Azure portal, edit it with Notepad, then copy the contents, paste it in **Download Certificate (Base64)** field.

	![Screenshot show selecting a certificate file and opening it with Notepad.](media/splashtop-tutorial/certificate.png)
	![Screenshot shows the contents of the certificate file.](media/splashtop-tutorial/file.png)
	![Screenshot shows the Download Certificate text box.](media/splashtop-tutorial/azure.png)

1. That's it! Click **Save** and Splashtop SSO validation team will contact you for the verification info, then activate the SSO method.

### Create Splashtop test user

1. After SSO method activated, please check the newly created SSO method to enable it in the **Single Sign On** section.

	![Screenshot shows the Single Sign On page where you can enable the new method.](media/splashtop-tutorial/enable.png)

1. Invite the test user, for example, `B.Simon@contoso.com` to your Splashtop team with the newly created SSO method.

	![Screenshot shows the Invite Users page where you can select your new method.](media/splashtop-tutorial/invite.png)

1. You can also change an existing Splashtop account to an SSO account, see [instructions](https://support-splashtopbusiness.splashtop.com/hc/en-us/articles/360038685691-How-to-associate-SSO-method-to-existing-team-admin-member-).

1. That's it! You can use the SSO account to log in Splashtop web portal or Splashtop Business app.

## Test SSO 

In this section, you test your Azure AD single sign-on configuration with following options. 

* Click on **Test this application** in Azure portal. This will redirect to Splashtop Sign-on URL where you can initiate the login flow. 

* Go to Splashtop Sign-on URL directly and initiate the login flow from there.

* You can use Microsoft My Apps. When you click the Splashtop tile in the My Apps, this will redirect to Splashtop Sign-on URL. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Splashtop you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

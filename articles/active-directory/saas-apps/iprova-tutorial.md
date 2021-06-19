---
title: 'Tutorial: Azure Active Directory single sign-on (SSO) integration with Zenya | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Zenya.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/08/2021
ms.author: jeedes
---

# Tutorial: Azure Active Directory single sign-on (SSO) integration with Zenya

In this tutorial, you'll learn how to integrate Zenya with Azure Active Directory (Azure AD). When you integrate Zenya with Azure AD, you can:

* Control in Azure AD who has access to Zenya.
* Enable your users to be automatically signed-in to Zenya with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Zenya single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD SSO in a test environment.

* Zenya supports **SP** initiated SSO.

## Add Zenya from the gallery

To configure the integration of Zenya into Azure AD, you need to add Zenya from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Zenya** in the search box.
1. Select **Zenya** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Zenya

Configure and test Azure AD SSO with Zenya using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Zenya.

To configure and test Azure AD SSO with Zenya, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
    1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
    1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Zenya SSO](#configure-zenya-sso)** - to configure the single sign-on settings on application side.
    1. **[Create Zenya test user](#create-zenya-test-user)** - to have a counterpart of B.Simon in Zenya that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Retrieve configuration information from Zenya

In this section, you retrieve information from Zenya to configure Azure AD single sign-on.

1. Open a web browser, and go to the **SAML2 info** page in Zenya by using the following URL patterns:
	
     `https://<SUBDOMAIN>.iprova.nl/saml2info` 
  	 `https://<SUBDOMAIN>.iprova.be/saml2info`
	 `https://<SUBDOMAIN>.iprova.eu/saml2info` 

	![View the Zenya SAML2 info page](media/iprova-tutorial/information.png)

1. Leave the browser tab open while you proceed with the next steps in another browser tab.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Zenya** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

1. On the **Basic SAML Configuration** section, perform the following steps:

	a. Fill the **Sign-on URL** box with the value that's displayed behind the label **Sign-on URL** on the **Zenya SAML2 info** page. This page is still open in your other browser tab.

	b. Fill the **Identifier** box with the value that's displayed behind the label **EntityID** on the **Zenya SAML2 info** page. This page is still open in your other browser tab.

	c. Fill the **Reply-URL** box with the value that's displayed behind the label **Reply URL** on the **Zenya SAML2 info** page. This page is still open in your other browser tab.

1. Zenya application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration. The following screenshot shows the list of default attributes.

	![image](common/default-attributes.png)

1. In addition to above, Zenya application expects few more attributes to be passed back in SAML response which are shown below. These attributes are also pre populated but you can review them as per your requirements.

	| Name | Source Attribute| Namespace  |
	| ---------------| -------- | -----|
	| `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|

1. On the **Set up single sign-on with SAML** page, In the **SAML Signing Certificate** section, click copy button to copy **App Federation Metadata Url** and save it on your computer.

	![The Certificate download link](common/copy-metadataurl.png)

## Create an Azure AD test user

In this section, you'll create a test user in the Azure portal called B.Simon.

1. From the left pane in the Azure portal, select **Azure Active Directory**, select **Users**, and then select **All users**.
1. Select **New user** at the top of the screen.
1. In the **User** properties, follow these steps:
   1. In the **Name** field, enter `B.Simon`.  
   1. In the **User name** field, enter the username@companydomain.extension. For example, `B.Simon@contoso.com`.
   1. Select the **Show password** check box, and then write down the value that's displayed in the **Password** box.
   1. Click **Create**.

## Assign the Azure AD test user

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Zenya.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Zenya**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you are expecting a role to be assigned to the users, you can select it from the **Select a role** dropdown. If no role has been set up for this app, you see "Default Access" role selected.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Zenya SSO

1. Sign in to Zenya by using the **Administrator** account.

2. Open the **Go to** menu.

3. Select **Application management**.

4. Select **General** in the **System settings** panel.

5. Select **Edit**.

6. Scroll down to **Access control**.

	![Zenya Access control settings](media/iprova-tutorial/access-control.png)

7. Find the setting **Users are automatically logged on with their network accounts**, and change it to **Yes, authentication via SAML**. Additional options now appear.

8. Select **Set up**.

9. Select **Next**.

10. Zenya asks if you want to download federation data from a URL or upload it from a file. Select the **From URL** option.

	![Download Azure AD metadata](media/iprova-tutorial/metadata.png)

11. Paste the metadata URL you saved in the last step of the "Configure Azure AD single sign-on" section.

12. Select the arrow-shaped button to download the metadata from Azure AD.

13. When the download is complete, the confirmation message **Valid Federation Data file downloaded** appears.

14. Select **Next**.

15. Skip the **Test login** option for now, and select **Next**.

16. In the **Claim to use** drop-down box, select **windowsaccountname**.

17. Select **Finish**.

18. You now return to the **Edit general settings** screen. Scroll down to the bottom of the page, and select **OK** to save your configuration.

## Create Zenya test user

1. Sign in to Zenya by using the **Administrator** account.

2. Open the **Go to** menu.

3. Select **Application management**.

4. Select **Users** in the **Users and user groups** panel.

5. Select **Add**.

6. In the **Username** box, enter the username of user like `B.Simon@contoso.com`.

7. In the **Full name** box, enter a full name of user like **B.Simon**.

8. Select the **No password (use single sign-on)** option.

9. In the **E-mail address** box, enter the email address of user like `B.Simon@contoso.com`.

10. Scroll down to the end of the page, and select **Finish**.

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options. 

* Click on **Test this application** in Azure portal. This will redirect to Zenya Sign-on URL where you can initiate the login flow. 

* Go to Zenya Sign-on URL directly and initiate the login flow from there.

* You can use Microsoft My Apps. When you click the Zenya tile in the My Apps, this will redirect to Zenya Sign-on URL. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Zenya you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

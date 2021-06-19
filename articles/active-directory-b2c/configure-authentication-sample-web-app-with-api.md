---
title: Configure authentication in a sample web application that calls a web API using Azure Active Directory B2C
description:  Using Azure Active Directory B2C to sign in and sign up users in an ASP.NET web application that calls a web API.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/11/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: "b2c-support"
---

# Configure authentication in a sample web application that calls a web API using Azure Active Directory B2C

This article uses a sample ASP.NET web application that calls a web API to illustrate how to add Azure Active Directory B2C (Azure AD B2C) authentication to your web applications.

> [!IMPORTANT]
> The sample ASP.NET web application referenced in this article is used to call a web API with a bearer token. For a web application that doesn't call a web API, see [Configure authentication in a sample web application using Azure Active Directory B2C](configure-authentication-sample-web-app.md).  

## Overview

OpenID Connect (OIDC) is an authentication protocol built on OAuth 2.0 that you can use to securely sign a user in to an application. This web app sample uses [Microsoft Identity Web](https://www.nuget.org/packages/Microsoft.Identity.Web). Microsoft Identity Web is a set of ASP.NET Core libraries that simplifies adding authentication and authorization support to web apps that can call a secure web API. 

The sign-in flow involves following steps:

1. The user navigates to the web app and select **Sign-in**.
1. The app initiates authentication request, and redirects the user to Azure AD B2C.
1. The user [signs-up or signs-in](add-sign-up-and-sign-in-policy.md), [resets the password](add-password-reset-policy.md), or signs-in with a [social account](add-identity-provider.md).
1. Upon successful sign-in, Azure AD B2C returns an authorization code to the app.
1. The app takes the following actions
    1. Exchanges the authorization code to an ID token, access token and refresh token.
    1. Reads the ID token claims, and persists an application authorization cookie.
    1. Stores the refresh token in an in-memory cache for later use.

### App registration overview

To enable your app to sign in with Azure AD B2C and call a web API, you register two applications in the Azure AD B2C directory.  

- The **web application** registration enables your app to sign in with Azure AD B2C. During app registration, you specify the *Redirect URI*. The redirect URI is the endpoint to which the user is redirected by Azure AD B2C after they authenticate with Azure AD B2C is completed. The app registration process generates an *Application ID*, also known as the *client ID*, that uniquely identifies your app. You also create a *client secret*, which is used by your application to securely acquire the tokens.

- The  **web API** registration enables your app to call a secure web API. The registration includes the web API *scopes*. The scopes provide a way to manage permissions to protected resources, such as your web API. You grant the web application permissions to the web API scopes. When an access token is requested, your app specifies the desired permissions in the scope parameter of the request.  

The following diagrams describe the apps registration and the application architecture.

![Web app with web API call registrations and tokens](./media/configure-authentication-sample-web-app-with-api/web-app-with-api-architecture.png) 

### Call to a web API

[!INCLUDE [active-directory-b2c-app-integration-call-api](../../includes/active-directory-b2c-app-integration-call-api.md)]

### Sign-out

[!INCLUDE [active-directory-b2c-app-integration-sign-out-flow](../../includes/active-directory-b2c-app-integration-sign-out-flow.md)]

## Prerequisites

A computer that's running either: 

# [Visual Studio](#tab/visual-studio)

* [Visual Studio 2019 16.8 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload
* [.NET 5.0 SDK](https://dotnet.microsoft.com/download/dotnet)

# [Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# for Visual Studio Code (latest version)](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET 5.0 SDK](https://dotnet.microsoft.com/download/dotnet)

---

## Step 1: Configure your user flow

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](../../includes/active-directory-b2c-app-integration-add-user-flow.md)]

## Step 2: Register web applications

In this step, you create the web app and the web API application registration, and specify the scopes of your web API.

### 2.1 Register the web API app

[!INCLUDE [active-directory-b2c-app-integration-register-api](../../includes/active-directory-b2c-app-integration-register-api.md)]

### 2.2 Configure web API app scopes

[!INCLUDE [active-directory-b2c-app-integration-api-scopes](../../includes/active-directory-b2c-app-integration-api-scopes.md)]


### 2.3 Register the web app

Follow these steps to create the web app registration:

1. Select **App registrations**, and then select **New registration**.
1. Enter a **Name** for the application. For example, *webapp1*.
1. Under **Supported account types**, select **Accounts in any identity provider or organizational directory (for authenticating users with user flows)**. 
1. Under **Redirect URI**, select **Web**, and then enter `https://localhost:5000/signin-oidc` in the URL text box.
1. Under **Permissions**, select the **Grant admin consent to openid and offline access permissions** check box.
1. Select **Register**.
1. After the app registration is completed, select **Overview**.
1. Record the **Application (client) ID** for use in a later step when you configure the web application.

    ![Get your web application ID](./media/configure-authentication-sample-web-app-with-api/get-azure-ad-b2c-app-id.png)  

### 2.4 Create a web app client secret

[!INCLUDE [active-directory-b2c-app-integration-client-secret](../../includes/active-directory-b2c-app-integration-client-secret.md)]


### 2.5 Grant the web app permissions for the web API

[!INCLUDE [active-directory-b2c-app-integration-grant-permissions](../../includes/active-directory-b2c-app-integration-grant-permissions.md)]

## Step 3: Get the web app sample

[Download the zip file](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/refs/heads/master.zip), or clone the sample web application from GitHub. 

```bash
git clone https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2
```

Extract the sample file to a folder where the total character length of the path is less than 260.

## Step 4: Configure the sample web API

In the sample folder, under the `4-WebApp-your-API/4-2-B2C/TodoListService` folder, open the **TodoListService.csproj** project with Visual Studio or Visual Studio Code. 

Under the project root folder, open the `appsettings.json` file. This file contains information about your Azure AD B2C identity provider. The web API app uses this information to validate the access token the web app passes as a bearer token. Update the following properties of the app settings:

|Section  |Key  |Value  |
|---------|---------|---------|
|AzureAdB2C|Instance| The first part of your Azure AD B2C [tenant name](tenant-management.md#get-your-tenant-name). For example, `https://contoso.b2clogin.com`.|
|AzureAdB2C|Domain| Your Azure AD B2C tenant full [tenant name](tenant-management.md#get-your-tenant-name). For example, `contoso.onmicrosoft.com`.|
|AzureAdB2C|ClientId| The web API application ID from [step 2.1](#21-register-the-web-api-app).|
|AzureAdB2C|SignUpSignInPolicyId|The user flows, or custom policy you created in [step 1](#step-1-configure-your-user-flow).|

Your final configuration file should look like the following JSON:

```json
{
  "AzureAdB2C": {
    "Instance": "https://contoso.b2clogin.com",
    "Domain": "contoso.onmicrosoft.com",
    "ClientId": "<web-api-app-application-id>",
    "SignedOutCallbackPath": "/signout/<your-sign-up-in-policy>",
    "SignUpSignInPolicyId": "<your-sign-up-in-policy>"
  },
  // More setting here
}
```

### 4.1 Set the permission policy

The web API verifies that the user authenticated with the bearer token, and the bearer token has the configured accepted scopes. If the bearer token does not have any of these accepted scopes, the web API returns HTTP status code 403 (Forbidden) and writes to the response body a message telling which scopes are expected in the token. 

To configure the accepted scopes, open the  `Controller/TodoListController.cs` class, and set the scope name. The scope name, without the full URI.

```csharp
[RequiredScope("tasks.read")]
```

### 4.2 Run the sample web API app

To allow web app calling the web API sample, follow these steps to run the web API:

1. If requested, restore dependencies.
1. Build and run the project.
1. After the project is built, Visual Studio or Visual Studio Code launches the web API in the browsers with the following address https://localhost:44332.

## Step 5: Configure the sample web app

In the sample folder, under the `4-WebApp-your-API/4-2-B2C/Client` folder, open the **TodoListClient.csproj** project with Visual Studio or Visual Studio Code. 

Under the project root folder, open the `appsettings.json` file. This file contains information about your Azure AD B2C identity provider. The web app uses this information to establish a trust relationship with Azure AD B2C, sign-in the user in and out, acquire tokens, and validate them. Update the following properties of the app settings:

|Section  |Key  |Value  |
|---------|---------|---------|
|AzureAdB2C|Instance| The first part of your Azure AD B2C [tenant name](tenant-management.md#get-your-tenant-name). For example, `https://contoso.b2clogin.com`.|
|AzureAdB2C|Domain| Your Azure AD B2C tenant full [tenant name](tenant-management.md#get-your-tenant-name). For example, `contoso.onmicrosoft.com`.|
|AzureAdB2C|ClientId| The web application ID from [step 2.1](#21-register-the-web-api-app).|
|AzureAdB2C | ClientSecret | The web application secret from [step 2.4](#24-create-a-web-app-client-secret). | 
|AzureAdB2C|SignUpSignInPolicyId|The user flows or custom policy you created in [step 1](#step-1-configure-your-user-flow).|
| TodoList | TodoListScope | The scopes you from [step 2.5](#25-grant-the-web-app-permissions-for-the-web-api).|
| TodoList | TodoListBaseAddress | The base URI of your web API, for example `https://localhost:44332`|

Your final configuration file should look like the following JSON:

```JSon
{
  "AzureAdB2C": {
    "Instance": "https://contoso.b2clogin.com",
    "Domain": "contoso.onmicrosoft.com",
    "ClientId": "<web-app-application-id>",
    "ClientSecret": "<web-app-application-secret>",  
    "SignedOutCallbackPath": "/signout/<your-sign-up-in-policy>",
    "SignUpSignInPolicyId": "<your-sign-up-in-policy>"
  },
  "TodoList": {
    "TodoListScope": "https://contoso.onmicrosoft.com/api/demo.read",
    "TodoListBaseAddress": "https://localhost:44332"
  }
}
```


## Step 6: Run the sample web app

1. Build and run the project.
1. Browse to https://localhost:5000. 
1. Complete the sign-up or sign-in process.

After successful authentication, you'll see your display name in the navigation bar. To view the claims that Azure AD B2C token returns to your app, select **TodoList**.

![Web app token's claims](./media/configure-authentication-sample-web-app-with-api/web-api-to-do-list.png)


## Deploy your application

In a production application, the app registration redirect URI is typically a publicly accessible endpoint where your app is running, like `https://contoso.com/signin-oidc`. 

You can add and modify redirect URIs in your registered applications at any time. The following restrictions apply to redirect URIs:

* The reply URL must begin with the scheme `https`.
* The reply URL is case-sensitive. Its case must match the case of the URL path of your running application. 

### Token cache for a web app

The web app sample uses in memory token cache serialization. This implementation is great in samples. It's also good in production applications provided you don't mind if the token cache is lost when the web app is restarted. 

For production environment, we recommend you use a distributed memory cache. For example, Redis cache, NCache, or a SQL Server cache. For details about the distributed memory cache implementations, see [Token cache for a web app](../active-directory/develop/msal-net-token-cache-serialization.md#token-cache-for-a-web-app-confidential-client-application).


## Next steps

* Learn more [about the code sample](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-5-B2C#about-the-code)
* Learn how to [Enable authentication in your own web application using Azure AD B2C](enable-authentication-web-application.md)

---
title: Enable web application  that calls a web API options using Azure Active Directory B2C
description:  Enable the use of web application  that calls a web API options by using several ways.
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

# Configure authentication in a sample web application  that calls a web API using Azure Active Directory B2C options

This article describes ways you can customize and enhance the Azure Active Directory B2C (Azure AD B2C) authentication experience for your web application that calls a web API. Before you start, familiarize yourself with the following articles: [Configure authentication in a sample web application](configure-authentication-sample-web-app-with-api.md) or [Enable authentication in your own web application](enable-authentication-web-app-with-api.md).

[!INCLUDE [active-directory-b2c-app-integration-custom-domain](../../includes/active-directory-b2c-app-integration-custom-domain.md)] 

To use a custom domain and your tenant ID in the authentication URL, follow the guidance in [Enable custom domains](custom-domain.md). Under the project root folder, open the `appsettings.json` file. This file contains information about your Azure AD B2C identity provider. 

- Update the `Instance` entry with your custom domain.
- Update the `Domain` entry with your [tenant ID](tenant-management.md#get-your-tenant-id). For more information, see [Use tenant ID](custom-domain.md#optional-use-tenant-id).

The following JSON shows the app settings before the change: 

```JSon
"AzureAdB2C": {
  "Instance": "https://contoso.b2clogin.com",
  "Domain": "tenant-name.onmicrosoft.com",
  ...
}
```  

The following JSON shows the app settings after the change: 

```JSon
"AzureAdB2C": {
  "Instance": "https://login.contoso.com",
  "Domain": "00000000-0000-0000-0000-000000000000",
  ...
}
``` 

## Support advanced scenarios

The `AddMicrosoftIdentityWebAppAuthentication` method in the Microsoft identity platform API lets developers add code for advanced authentication scenarios or subscribe to OpenIdConnect events. For example, you can subscribe to OnRedirectToIdentityProvider, which allows you to customize the authentication request your app sends to Azure AD B2C.

To support advanced scenarios, open the `Startup.cs`, and in the `ConfigureServices` function, replace the `AddMicrosoftIdentityWebAppAuthentication` with the following code snippet: 

```csharp
// Configuration to sign-in users with Azure AD B2C

//services.AddMicrosoftIdentityWebAppAuthentication(Configuration, "AzureAdB2C");

services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
        .AddMicrosoftIdentityWebApp(options =>
{
    Configuration.Bind("AzureAdB2C", options);
    options.Events ??= new OpenIdConnectEvents();
    options.Events.OnRedirectToIdentityProvider += OnRedirectToIdentityProviderFunc;
});
```

The code above adds the OnRedirectToIdentityProvider event with a reference to the *OnRedirectToIdentityProviderFunc* method. Add the following code snippet to the `Startup.cs` class.

```csharp
private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
{
    // Custom code here
    
    // Don't remove this line
    await Task.CompletedTask.ConfigureAwait(false);
}
```

You can pass parameters between your controller and the *OnRedirectToIdentityProvider* function using context parameters. 


[!INCLUDE [active-directory-b2c-app-integration-login-hint](../../includes/active-directory-b2c-app-integration-login-hint.md)]

1. If you're using a custom policy, add the required input claim as described in [Set up direct sign-in](direct-signin.md#prepopulate-the-sign-in-name). 
1. Complete the [Support advanced scenarios](#support-advanced-scenarios) procedure.
1. Add the following line of code to the *OnRedirectToIdentityProvider* function:
    
    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      context.ProtocolMessage.LoginHint = "emily@contoso.com";
      
      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```

[!INCLUDE [active-directory-b2c-app-integration-domain-hint](../../includes/active-directory-b2c-app-integration-domain-hint.md)]

1. Check the domain name of your external identity provider. For more information, see [Redirect sign-in to a social provider](direct-signin.md#redirect-sign-in-to-a-social-provider).
1. Complete the [Support advanced scenarios](#support-advanced-scenarios) procedure. 
1. In the *OnRedirectToIdentityProviderFunc* function, add the following line of code to the *OnRedirectToIdentityProvider* function:
    
    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      context.ProtocolMessage.DomainHint = "facebook.com";
      
      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```

[!INCLUDE [active-directory-b2c-app-integration-ui-locales](../../includes/active-directory-b2c-app-integration-ui-locales.md)]

1. [Configure Language customization](language-customization.md).
1. Complete the [Support advanced scenarios](#support-advanced-scenarios) procedure.
1. Add the following line of code to the *OnRedirectToIdentityProvider* function:

    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      context.ProtocolMessage.UiLocales = "es";

      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```
    ```

[!INCLUDE [active-directory-b2c-app-integration-custom-parameters](../../includes/active-directory-b2c-app-integration-custom-parameters.md)]

1. Configure the [ContentDefinitionParameters](customize-ui-with-html.md#configure-dynamic-custom-page-content-uri) element.
1. Complete the [Support advanced scenarios](#support-advanced-scenarios) procedure.
1. Add the following line of code to the *OnRedirectToIdentityProvider* function:
    
    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      context.ProtocolMessage.Parameters.Add("campaignId", "123");

      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```

[!INCLUDE [active-directory-b2c-app-integration-id-token-hint](../../includes/active-directory-b2c-app-integration-id-token-hint.md)]
 
1. Complete the [Support advanced scenarios](#support-advanced-scenarios) procedure.
1. In your custom policy, define an [ID token hint technical profile](id-token-hint.md).
1. Add the following line of code to the *OnRedirectToIdentityProvider* function:
    
    ```csharp
    private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
    {
      // The idTokenHint variable holds your ID token 
      context.ProtocolMessage.IdTokenHint = idTokenHint

      // More code
      await Task.CompletedTask.ConfigureAwait(false);
    }
    ```
    
## Account controller

If you want to customize the **Sign-in**, **Sign-up** or **Sign-out** actions, you are encouraged to create your own controller. Having your own controller allows you to pass parameters between your controller and the authentication library. The `AccountController` is part of `Microsoft.Identity.Web.UI`  NuGet package, which handles the sign-in and sign-out actions. You can find its implementation in the [Microsoft Identity Web library](https://github.com/AzureAD/microsoft-identity-web/blob/master/src/Microsoft.Identity.Web.UI/Areas/MicrosoftIdentity/Controllers/AccountController.cs). 

The following code snippet demonstrates a custom `MyAccountController` with the **SignIn** action. The action passes a parameter named `campaign_id` to the authentication library.

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.OpenIdConnect;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;


namespace mywebapp.Controllers
{
    [AllowAnonymous]
    [Area("MicrosoftIdentity")]
    [Route("[area]/[controller]/[action]")]
    public class MyAccountController : Controller
    {

        [HttpGet("{scheme?}")]
        public IActionResult SignIn([FromRoute] string scheme)
        {
            scheme ??= OpenIdConnectDefaults.AuthenticationScheme;
            var redirectUrl = Url.Content("~/");
            var properties = new AuthenticationProperties { RedirectUri = redirectUrl };
            properties.Items["campaign_id"] = "1234";
            return Challenge(properties, scheme);
        }

    }
}

```

In the `_LoginPartial.cshtml` view, change the sign-in link to your controller

```
<form method="get" asp-area="MicrosoftIdentity" asp-controller="MyAccount" asp-action="SignIn">
```

In the `OnRedirectToIdentityProvider` in the `Startup.cs` calls, you can read the custom parameter:

```csharp
private async Task OnRedirectToIdentityProviderFunc(RedirectContext context)
{
    // Read the custom parameter
    var campaign_id = (context.Properties.Items.ContainsKey("campaign_id"))
    
    // Add your custom code here
    
    await Task.CompletedTask.ConfigureAwait(false);
}
```

## Role-based access control

With [authorization in ASP.NET Core](/aspnet/core/security/authorization/introduction) you can use [role-based authorization](/aspnet/core/security/authorization/roles), [claims-based authorization](/aspnet/core/security/authorization/claims), or [policy-based authorization](/aspnet/core/security/authorization/policies) to check if the user is authorized to access a protected resource.

In the *ConfigureServices* method, add the *AddAuthorization* method, which adds the authorization model. The following example creates a policy named `EmployeeOnly`. The policy checks that a claim `EmployeeNumber` exists. The value of the claim must be one of the following IDs: 1, 2, 3, 4 or 5.

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy =>
              policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
```

Authorization in ASP.NET Core is controlled with [AuthorizeAttribute](/aspnet/core/security/authorization/simple) and its various parameters. In its most basic form, applying the `[Authorize]` attribute to a controller, action, or Razor Page, limits access to that component's authenticated users.

Policies are applied to controllers by using the `[Authorize]` attribute with the policy name. The following code limits access to the `Claims` action to  users authorized by the `EmployeeOnly` policy:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult Claims()
{
    return View();
}
```

## Next steps

- Learn more: [Introduction to authorization in ASP.NET Core](/aspnet/core/security/authorization/introduction)
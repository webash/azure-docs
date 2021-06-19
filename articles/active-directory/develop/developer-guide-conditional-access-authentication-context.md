---
title: Developer guidance for Azure AD Conditional Access authentication context
description: Developer guidance and scenarios for Azure AD Conditional Access authentication context

services: active-directory
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.date: 05/18/2021

ms.author: joflore
author: MicrosoftGuyJFlo
manager: CelesteDG
ms.reviewer: kkrishna

ms.workload: identity

ms.custom: aaddev
---
# Developers’ guide to Conditional Access authentication context

[Conditional Access](../conditional-access/overview.md) is the Zero Trust control plane that allows you to target policies for access to all your apps – old or new, private, or public, on-premises, or multi-cloud. With [Conditional Access authentication context](../conditional-access/concept-conditional-access-cloud-apps.md#authentication-context-preview), you can apply different policies within those apps. 

Conditional Access authentication context (auth context) allows you to apply granular policies to sensitive data and actions instead of just at the app level. You can refine your Zero Trust policies for least privileged access while minimizing user friction and keeping users more productive and your resources more secure. Today, it can be used by applications using [OpenId Connect](https://openid.net/specs/openid-connect-core-1_0.html) for authentication developed by your company to protect sensitive resources, like high-value transactions or viewing employee personal data.

Use the Azure AD Conditional access engine’s new auth context feature to trigger a demand for step-up authentication from within your application and services. Developers now have the power to demand enhanced stronger authentication, selectively, like MFA from their end users from within their applications. This feature helps developers build smoother user experiences for most parts of their application, while access to more secure operations and data remains behind stronger authentication controls.

## Problem statement

The IT administrators and regulators often struggle between balancing prompting their users with additional factors of authentication too frequently and achieving adequate security and policy adherence for applications and services where parts of them contain sensitive data and operations. It can be a choice between a strong policy that impacts users' productivity when they access most data and actions or a policy that is not strong enough for sensitive resources. 

So, what if apps were able to mix both, where they can function with a relatively lesser security and less frequent prompts for most users and operations and yet conditionally stepping up the security requirement when the users accessed more sensitive parts? 

## Common scenarios

For example, while users may sign in to SharePoint using multi-factor authentication, accessing site collection in SharePoint containing sensitive documents can require a compliant device and only be accessible from trusted IP ranges. 

## Steps

The following are the prerequisites and the steps if you want to use Conditional Access authentication context. 

### Prerequisites

**First**, your app should be integrated with the Microsoft Identity Platform using the use [OpenID Connect](v2-protocols-oidc.md)/ [OAuth 2.0](v2-oauth2-auth-code-flow.md) protocols for authentication and authorization. We recommend you use [Microsoft identity platform authentication libraries](reference-v2-libraries.md) to integrate and secure your application with Azure Active Directory. [Microsoft identity platform documentation](index.yml) is a good place to start learning how to integrate your apps with the Microsoft Identity Platform. Conditional Access Auth Context feature support is built on top of protocol extensions provided by the industry standard[OpenID Connect](v2-protocols-oidc.md) protocol. Developers use a [Conditional Access Auth Context reference](/graph/api/resources/authenticationcontextclassreference) **value** with the [Claims Request](claims-challenge.md) parameter to give apps a way to trigger and satisfy policy.

**Second**, [Conditional Access](../conditional-access/overview.md) requires Azure AD Premium P1 licensing. More information about licensing can be found on the [Azure AD pricing page](https://azure.microsoft.com/pricing/details/active-directory/).

**Third**, today it is only available to applications that sign-in users. Applications that authenticate as themselves are not supported. Use the [Authentication flows and application scenarios guide](authentication-flows-app-scenarios.md) to learn about the supported authentication app types and flows in the Microsoft Identity Platform.

### Integration steps

Once your application is integrated using the supported authentication protocols and registered in an Azure AD tenant that has the Conditional Access feature available for use, you can kick start the process to integrating this feature in your applications that sign-in users.

> [!NOTE]
> A detailed walkthrough of this feature is also available as a recorded session at [Use Conditional Access Auth Context in your app for step\-up authentication](https://www.youtube.com/watch?v=_iO7CfoktTY).

**First**, declare and make the authentication contexts available in your tenant. For more information, see [Configure authentication contexts](../conditional-access/concept-conditional-access-cloud-apps.md#configure-authentication-contexts).

Values **C1-C25** are available for use as **Auth Context IDs** in a tenant. Examples of auth context may be:

- **C1** - Require strong authentication
- **C2** – Require compliant devices
- **C3** – Require trusted locations

Create or modify your Conditional Access policies to use the Conditional Access Auth Contexts. Examples policies could be:

- All users signing-into this web application should have successfully completed 2FA for auth context ID **C1**.
- All users signing into this web application should have successfully completed 2FA and also access the web app from a certain IP address range for auth context ID **C3**.

> [!NOTE]
> The Conditional Access auth context values are declared and maintained separately from applications. It is not advisable for applications to take hard dependency on auth context ids. The Conditional Access policies are usually crafted by IT administrators as they have a better understanding of the resources available to apply policies on. For example, for an Azure AD tenant, IT admins would have the knowledge of how many of the tenant’s users are equipped to use 2FA for MFA and thus can ensure that Conditional Access policies that require 2FA are scoped to these equipped users. 
> Similarly, if the application is used in multiple tenants, the auth context ids in use could be different and, in some cases, not available at all.

**Second**: The developers of an application planning to use Conditional Access auth context are advised to first provide the application admins or IT admins a means to map potential sensitive actions to auth context IDs. The steps roughly being:

1. Identity actions in the code that can be made available to map against auth context Ids.
1. Build a screen in the admin portal of the app (or an equivalent functionality) that IT admins can use to map sensitive actions against an available auth context ID.
1. See the code sample, [Use the Conditional Access Auth Context to perform step-up authentication](https://github.com/Azure-Samples/ms-identity-ca-auth-context/blob/main/README.md) for an example on how it is done.

These steps are the changes that you need to carry in your code base. The steps broadly comprise of

- Query MS Graph to [list all the available Auth Contexts](/graph/api/conditionalaccessroot-list-authenticationcontextclassreferences).
- Allow IT admins to select sensitive/ high-privileged operations and assign them against the available Auth Contexts using CA policies. 
- Save this mapping information in your database/local store.

:::image type="content" source="media/developer-guide-conditional-access-authentication-context/configure-conditional-access-authentication-context.png" alt-text="Setup flow for creating an authentication context":::

**Third**: Your application, and for this example, we’d assume it’s a web API, then needs to evaluate calls against the saved mapping and accordingly raise claim challenges for its client apps. To prepare for this action, the following steps are to be taken:

1. In a sensitive and protected by auth context operation, evaluate the values in the **acrs** claim against the Auth Context ID mappings saved earlier and raise a [Claims Challenge](claims-challenge.md) as provided in the code snippet below. 

1. The following diagram shows the interaction between the user, client app, and the web API.

   :::image type="content" source="media/developer-guide-conditional-access-authentication-context/authentication-context-application-flow.png" alt-text="Diagram showing the interaction of user, web app, API, and Azure AD":::

   The code snippet that follows is from the code sample, [Use the Conditional Access auth context to perform step-up authentication](https://github.com/Azure-Samples/ms-identity-ca-auth-context/blob/main/README.md). The first method, `CheckForRequiredAuthContext()` in the API 

      - Checks if the application's action being called requires step-up authentication. It does so by checking its database for a saved mapping for this method
      - If this action indeed requires an elevated auth context, it checks the **acrs** claim for an existing, matching Auth Context ID.
      - If a matching Auth Context ID is not found, it raises a [claims challenge](claims-challenge.md#claims-challenge-header-format).

      ```
      public void CheckForRequiredAuthContext(string method)
      {
          string authType = _commonDBContext.AuthContext.FirstOrDefault(x => x.Operation == method 
                      && x.TenantId == _configuration["AzureAD:TenantId"])?.AuthContextId;

          if (!string.IsNullOrEmpty(authType))
          {
              HttpContext context = this.HttpContext;
              string authenticationContextClassReferencesClaim = "acrs";

              if (context == null || context.User == null || context.User.Claims == null 
                  || !context.User.Claims.Any())
              {
                  throw new ArgumentNullException("No Usercontext is available to pick claims from");
              }

              Claim acrsClaim = context.User.FindAll(authenticationContextClassReferencesClaim).FirstOrDefault(x 
                  => x.Value == authType);

              if (acrsClaim == null || acrsClaim.Value != authType)
              {
                  if (IsClientCapableofClaimsChallenge(context))
                  {
                      string clientId = _configuration.GetSection("AzureAd").GetSection("ClientId").Value;
                      var base64str = Convert.ToBase64String(Encoding.UTF8.GetBytes("{\"access_token\":{\"acrs\":{\"essential\":true,\"value\":\"" + authType + "\"}}}"));

                      context.Response.Headers.Append("WWW-Authenticate", $"Bearer realm=\"\", authorization_uri=\"https://login.microsoftonline.com/common/oauth2/authorize\", client_id=\"" + clientId + "\", error=\"insufficient_claims\", claims=\"" + base64str + "\", cc_type=\"authcontext\"");
                      context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
                      string message = string.Format(CultureInfo.InvariantCulture, "The presented access tokens had insufficient claims. Please request for claims requested in the WWW-Authentication header and try again.");
                      context.Response.WriteAsync(message);
                      context.Response.CompleteAsync();
                      throw new UnauthorizedAccessException(message);
                  }
                  else
                  {
                      throw new UnauthorizedAccessException("The caller does not meet the authentication  bar to carry our this operation. The service cannot allow this operation");
                  }
              }
          }
      }
      ```

   > [!NOTE]
   > The format of the claims challenge is described in the article, [Claims Challenge in the Microsoft Identity Platform](claims-challenge.md). 

1. In the client application, Intercept the claims challenge and redirect the user back to Azure AD for further policy evaluation. The code snippet that follows is from the code sample, [Use the Conditional Access auth context to perform step-up authentication](https://github.com/Azure-Samples/ms-identity-ca-auth-context/blob/main/README.md).

   ```
   internal static string ExtractHeaderValues(WebApiMsalUiRequiredException response)
   {
       if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized && response.Headers.WwwAuthenticate.Any())
       {
           AuthenticationHeaderValue bearer = response.Headers.WwwAuthenticate.First(v => v.Scheme == "Bearer");
           IEnumerable<string> parameters = bearer.Parameter.Split(',').Select(v => v.Trim()).ToList();
           var errorValue = GetParameterValue(parameters, "error");

           try
           {
               // read the header and checks if it conatins error with insufficient_claims value.
               if (null != errorValue && "insufficient_claims" == errorValue)
               {
                   var claimChallengeParameter = GetParameterValue(parameters, "claims");
                   if (null != claimChallengeParameter)
                   {
                       var claimChallenge = ConvertBase64String(claimChallengeParameter);

                       return claimChallenge;
                   }
               }
           }
           catch (Exception ex)
           {
               throw ex;
           }
       }
       return null;
   }
   ```

   Handle exception in the call to Web API, if a claims challenge is presented, the redirect the user back to Azure AD for further processing.

   ```
   try
   {
       // Call the API 
       await _todoListService.AddAsync(todo);
   }
   catch (WebApiMsalUiRequiredException hex)
   {
       // Challenges the user if exception is thrown from Web API.
       try
       {
           var claimChallenge =ExtractHeaderValues(hex);
           _consentHandler.ChallengeUser(new string[] { "user.read" }, claimChallenge);

           return new EmptyResult();
       }
       catch (Exception ex)
       {
           _consentHandler.HandleException(ex);
       }

       Console.WriteLine(hex.Message);
   }
   return RedirectToAction("Index");
   ```
   
1. (Optional) Declare client capability. Client capabilities help resources providers (RP) like our Web API above to detect if the calling client application understands the claims challenge and can then customize its response accordingly. This capability could be useful where not all of the APIs clients are capable of handling claim challenges and some older ones still expect a different response. For more information, see the section [Client capabilities](claims-challenge.md#client-capabilities).

## Caveats and recommendations

Do not hard-code Auth Context values in your app. Apps should read and apply auth context [using MS Graph calls](/graph/api/resources/authenticationcontextclassreference). This practice is critical for [multi-tenant applications](howto-convert-app-to-be-multi-tenant.md). The Auth Context values will vary between Azure AD tenants will not available in Azure AD free edition. For more information on how an app should query, set, and use auth context in their code, see the code sample, [Use the Conditional Access auth context to perform step-up authentication](https://github.com/Azure-Samples/ms-identity-ca-auth-context/blob/main/README.md) as how an app should query, set and use auth context in their code. 

Do not use auth context where the app itself is going to be a target of Conditional Access policies. The feature works best when parts of the application require the user to meet a higher bar of authentication.

## Next steps

- [Granular Conditional Access for sensitive data and actions (Blog)](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/granular-conditional-access-for-sensitive-data-and-actions/ba-p/1751775)
- [Zero trust with the Microsoft Identity platform](/security/zero-trust/identity-developer)
- [Building Zero Trust ready apps with the Microsoft identity platform](/security/zero-trust/identity-developer)
- [Use the Conditional Access auth context to perform step-up authentication for high-privilege operations in a Web API](https://github.com/Azure-Samples/ms-identity-ca-auth-context/blob/main/README.md)
- [Conditional Access authentication context](../conditional-access/concept-conditional-access-cloud-apps.md#authentication-context-preview)
- [authenticationContextClassReference resource type - MS Graph](/graph/api/conditionalaccessroot-list-authenticationcontextclassreferences)
- [Claims challenge, claims request, and client capabilities in the Microsoft Identity Platform](claims-challenge.md)
- [Using authentication context with Microsoft Information Protection and SharePoint](/microsoft-365/compliance/sensitivity-labels-teams-groups-sites#more-information-about-the-dependencies-for-the-authentication-context-option)
- [Authentication flows and application scenarios](authentication-flows-app-scenarios.md)
- [How to use Continuous Access Evaluation enabled APIs in your applications](app-resilience-continuous-access-evaluation.md)

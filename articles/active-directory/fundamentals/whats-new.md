---
title: What's new? Release notes - Azure Active Directory | Microsoft Docs
description: Learn what is new with Azure Active Directory; such as the latest release notes, known issues, bug fixes, deprecated functionality, and upcoming changes.
services: active-directory
author: ajburnle
manager: daveba
featureFlags:
 - clicktale
 
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 5/31/2021
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
---

# What's new in Azure Active Directory?

>Get notified about when to revisit this page for updates by copying and pasting this URL: `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` into your ![RSS feed reader icon](./media/whats-new/feed-icon-16x16.png) feed reader.

Azure AD receives improvements on an ongoing basis. To stay up to date with the most recent developments, this article provides you with information about:

- The latest releases
- Known issues
- Bug fixes
- Deprecated functionality
- Plans for changes

This page is updated monthly, so revisit it regularly. If you're looking for items older than six months, you can find them in [Archive for What's new in Azure Active Directory](whats-new-archive.md).

---

## May 2021

### Public preview - Azure AD verifiable credentials

**Type:** New feature  
**Service category:** Other  
**Product capability:** User Authentication
 
Azure AD customers can now easily design and issue verifiable credentials to represent proof of employment, education, or any other claim while respecting privacy. Digitally validate any piece of information about anyone and any business. [Learn more](../verifiable-credentials/index.yml).

---

### Public Preview - Device code flow now includes an app verification prompt

**Type:** New feature  
**Service category:** User Authentication  
**Product capability:** Authentications (Logins)
 
As a security improvement, the [device code flow](../develop/v2-oauth2-device-code.md) has been updated to include an additional prompt, which validates that the user is signing into the app they expect. The roll roll out is planned to start in June and expected to be complete by June 30.

To help prevent phishing attacks where an attacker tricks the user into signing into a malicious application, the following prompt is being added: “Are you trying to sign in to [application display name]?". All users will see this prompt while signing in using the device code flow. As a security measure, it cannot be removed or bypassed. [Learn more](../develop/reference-breaking-changes.md#the-device-code-flow-ux-will-now-include-an-app-confirmation-prompt).

---

### Public Preview - build and test expressions for user provisioning

**Type:** New feature  
**Service category:** App Provisioning  
**Product capability:** Identity Lifecycle Management
 
The expression builder allows you to create and test expressions, without having to wait for the full sync cycle. [Learn more](../app-provisioning/functions-for-customizing-application-data.md). 

---

### Public preview - enhanced audit logs for Conditional Access policy changes

**Type:** New feature  
**Service category:** Conditional Access  
**Product capability:** Identity Security & Protection
 
An important aspect of managing Conditional Access is understanding changes to your policies over time. Policy changes may cause disruptions for your end users, so maintaining a log of changes and enabling admins to revert to previous policy versions is critical. 

In addition to showing who made a policy change and when, the audit logs will now also contain a modified properties value so that admins have greater visibility into what assignments, conditions, or controls changed. If you want to revert to a previous version of a policy, you can copy the JSON representation of the old version and use the Conditional Access APIs to quickly change the policy back to its previous state. [Learn more](../conditional-access/concept-conditional-access-policies.md).

---

### Public preview - Sign-in logs include authentication methods used during sign-in

**Type:** New feature  
**Service category:** MFA  
**Product capability:** Monitoring & Reporting
 

Admins can now see the sequential steps users took to sign-in, including which authentication methods were used during sign-in. 

To access these details, go to the Azure AD sign-in logs, select a sign-in, and then navigate to the Authentication Method Details tab. Here we have included information such as which method was used, details about the method (e.g. phone number, phone name), authentication requirement satisfied, and result details. [Learn more](../reports-monitoring/concept-sign-ins.md).

---

### Public preview - PIM adds support for ABAC conditions in Azure Storage roles

**Type:** New feature  
**Service category:** Privileged Identity Management  
**Product capability:** Privileged Identity Management
 
Along with the public preview of attributed based access control for specific Azure RBAC role, you can also add ABAC conditions inside Privileged Identity Management for your eligible assignments. [Learn more](../../role-based-access-control/conditions-overview.md#conditions-and-privileged-identity-management-pim).

---

### General availability - Conditional Access and Identity Protection Reports in B2C

**Type:** New feature  
**Service category:** B2C - Consumer Identity Management  
**Product capability:** B2B/B2C

B2C now supports Conditional Access and Identity Protection for business-to-consumer (B2C) apps and users. This enables customers to protect their users with granular risk- and location-based access controls. With these features, customers can now look at the signals and create a policy to provide more security and access to your customers. [Learn more](../../active-directory-b2c/conditional-access-identity-protection-overview.md).

---

### General availability - KMSI and Password reset now in next generation of user flows

**Type:** New feature  
**Service category:** B2C - Consumer Identity Management  
**Product capability:** B2B/B2C

The next generation of B2C user flows now supports [keep me signed in (KMSI)](../../active-directory-b2c/session-behavior.md?pivots=b2c-custom-policy#enable-keep-me-signed-in-kmsi) and password reset. The KMSI functionality allows customers to extend the session lifetime for the users of their web and native applications by using a persistent cookie. This feature keeps the session active even when the user closes and reopens the browser, and is revoked when the user signs out. Password reset allows users to reset their password from the "Forgot your password
' link. This also allows the admin to force reset the user's expired password in the Azure AD B2C directory. [Learn more](../../active-directory-b2c/add-password-reset-policy.md?pivots=b2c-user-flow).
 
---

### General availability - New Log Analytics workbook Application role assignment activity

**Type:** New feature  
**Service category:** User Access Management  
**Product capability:** Entitlement Management
 
A new workbook has been added for surfacing audit events for application role assignment changes. [Learn more](../governance/entitlement-management-logs-and-reporting.md).

---

### General availability - Next generation Azure AD B2C user flows 

**Type:** New feature  
**Service category:** B2C - Consumer Identity Management  
**Product capability:** B2B/B2C
 
The new simplified user flow experience offers feature parity with preview features and is the home for all new features. Users will be able to enable new features within the same user flow, reducing the need to create multiple versions with every new feature release. The new, user-friendly UX also simplifies the selection and creation of user flows. Refer to [Create user flows in Azure AD B2C](../../active-directory-b2c/tutorial-create-user-flows.md?pivots=b2c-user-flow) for guidance on using this feature. [Learn more](../../active-directory-b2c/user-flow-versions.md).

---

### General availability - Azure Active Directory threat intelligence for sign-in risk

**Type:** New feature  
**Service category:** Identity Protection  
**Product capability:** Identity Security & Protection
 
This new detection serves as an ad-hoc method to allow our security teams to notify you and protect your users by raising their session risk to a High risk when we observe an attack happening, as well as marking the associated sign-ins as risky. This detection follows the existing Azure Active Directory threat intelligence for user risk detection to provide complete coverage of the various attacks observed by Microsoft security teams. [Learn more](../identity-protection/concept-identity-protection-risks.md#user-risk).
 
---

### General availability - Conditional Access named locations improvements

**Type:** New feature  
**Service category:** Conditional Access  
**Product capability:** Identity Security & Protection
 
IPv6 support in named locations is now generally available. Updates include:

- Added the capability to define IPv6 address ranges
- Increased limit of named locations from 90 to 195
- Increased limit of IP ranges per named location from 1200 to 2000
- Added capabilities to search and sort named locations and filter by location type and trust type
- Added named locations a sign-in belonged to in the sign-in logs
 
Additionally, to prevent admins from defining problematic named locations, additional checks have been added to reduce the chance of misconfiguration. [Learn more](../conditional-access/location-condition.md).

---

### General Availability - Restricted guest access permissions in Azure AD

**Type:** New feature  
**Service category:** User Management  
**Product capability:** Directory
 
Directory level permissions for guest users have been updated. These permissions allow administrators to require additional restrictions and controls on external guest user access.

Admins can now add additional restrictions for external guests' access to user and groups' profile and membership information. Also, customers can manage external user access at scale by hiding group memberships, including restricting guest users from seeing memberships of the group(s) they are in. To learn more, see [Restrict guest access permissions in Azure Active Directory](../enterprise-users/users-restrict-guest-permissions.md).
 
---

### New Federated Apps available in Azure AD Application gallery - May 2021

**Type:** New feature  
**Service category:** Enterprise Apps  
**Product capability:** 3rd Party Integration
 
You can now automate creating, updating, and deleting user accounts for these newly integrated apps:

- [AuditBoard](../saas-apps/auditboard-provisioning-tutorial.md)
- [Cisco Umbrella User Management](../saas-apps/cisco-umbrella-user-management-provisioning-tutorial.md)
- [Insite LMS](../saas-apps/insite-lms-provisioning-tutorial.md)
- [kpifire](../saas-apps/kpifire-provisioning-tutorial.md)
- [UNIFI](../saas-apps/unifi-provisioning-tutorial.md)

For more information about how to better secure your organization using automated user account provisioning, see [Automate user provisioning to SaaS applications with Azure AD](../app-provisioning/user-provisioning.md).

---

### New Federated Apps available in Azure AD Application gallery - May 2021

**Type:** New feature  
**Service category:** Enterprise Apps  
**Product capability:** 3rd Party Integration
 
In May 2021 we have added following 29 new applications in our App gallery with Federation support

[InviteDesk](https://app.invitedesk.com/login), [Webrecruit ATS](https://id-test.webrecruit.co.uk/), [Workshop](../saas-apps/workshop-tutorial.md), [Gravity Sketch](https://landingpad.me/), [JustLogin](../saas-apps/justlogin-tutorial.md), [Custellence](https://custellence.com/sso/), [WEVO](https://hello.wevoconversion.com/login), [AppTec360 MDM](https://www.apptec360.com/ms/autopilot.html), [Filemail](https://www.filemail.com/login),[Ardoq](../saas-apps/ardoq-tutorial.md), [Leadfamly](../saas-apps/leadfamly-tutorial.md), [Documo](../saas-apps/documo-tutorial.md), [Autodesk SSO](../saas-apps/autodesk-sso-tutorial.md), [Check Point Harmony Connect](../saas-apps/check-point-harmony-connect-tutorial.md), [BrightHire](https://app.brighthire.ai/), [Rescana](../saas-apps/rescana-tutorial.md), [Bluewhale](https://cloud.bluewhale.dk/), [AlacrityLaw](../saas-apps/alacritylaw-tutorial.md), [Equisolve](../saas-apps/equisolve-tutorial.md), [Zip](../saas-apps/zip-tutorial.md), [Cognician](../saas-apps/cognician-tutorial.md), [Acra](https://www.acrasuite.com/), [VaultMe](https://app.vaultme.com/#/signIn), [TAP App Security](../saas-apps/tap-app-security-tutorial.md), [Cavelo Office365 Cloud Connector](https://dashboard.prod.cavelodata.com/), [Clebex](../saas-apps/clebex-tutorial.md), [Banyan Command Center](../saas-apps/banyan-command-center-tutorial.md), [Check Point Remote Access VPN](../saas-apps/check-point-remote-access-vpn-tutorial.md), [LogMeIn](../saas-apps/logmein-tutorial.md)

You can also find the documentation of all the applications from here https://aka.ms/AppsTutorial

For listing your application in the Azure AD app gallery, read the details here https://aka.ms/AzureADAppRequest

---

### Improved Conditional Access Messaging for Android and iOS

**Type:** Changed feature  
**Service category:** Device Registration and Management  
**Product capability:** End User Experiences
 

We have updated the wording on the Conditional Access screen shown to users when they are blocked from accessing corporate resources until they enroll their device in Mobile Device Management. These improvements apply to the Android and iOS/iPadOS platforms. The following have been changed:

- “Help us keep your device secure” has changed to “Set up your device to get access”
- “Your sign-in was successful but your admin requires your device to be managed by Microsoft to access this resource.” to “[Organization’s name] requires you to secure this device before you can access [organization’s name] email, files, and data.” 
- “Enroll Now” to “Continue”

Note that the information in [Enroll your Android enterprise device](https://support.microsoft.com/topic/enroll-your-android-enterprise-device-d661c82d-fa28-5dfd-b711-6dff41ae83bb) is out of date.

---

### Azure Information Protection service will begin asking for consent

**Type:** Changed feature  
**Service category:** Authentications (Logins)  
**Product capability:** User Authentication
 
The Azure Information Protection service signs users into the tenant that encrypted the document as part of providing access to the document.  Starting June, Azure AD will begin prompting the user for consent when this access is performed across organizations.  This ensures that the user understands that the organization which owns the document will collect some information about the user as part of the document access. [Learn more](/azure/information-protection/known-issues#sharing-external-doc-types-across-tenants).
 
---

### Provisioning logs schema change impacting Graph API and Azure Monitor integration

**Type:** Changed feature  
**Service category:** App Provisioning  
**Product capability:** Monitoring & Reporting
 

The attributes "Action" and "statusInfo" will be changed to "provisioningAction" and "provisoiningStatusInfo." Please update any scripts that you have created using the [provisioning logs Graph API](/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta&preserve-view=true) or [Azure Monitor integrations](../app-provisioning/application-provisioning-log-analytics.md). 
 

---

### New ARM API to manage PIM for Azure Resources and Azure AD roles

**Type:** Changed feature  
**Service category:** Privileged Identity Management  
**Product capability:** Privileged Identity Management
 
An updated version of PIM's API for Azure Resource role and Azure AD role has been released. The PIM API for Azure Resource role is now released under the ARM API standard which aligns with the role management API for regular Azure role assignment. On the other hand, the PIM API for Azure AD roles is also released under graph API aligned with the unifiedRoleManagement APIs. Some of the benefit of this change include:

- Alignment of the PIM API with objects in ARM and Graph for role managementReducing the need to call PIM to onboard new Azure resources. 
- All Azure resources automatically work with new PIM API.
- Reducing the need to call PIM for role definition or keeping a PIM resource ID
- Supporting app-only API permissions in PIM for both Azure AD and Azure Resource roles

Previous version of PIM's API under /privilegedaccess will continue to function but we recommend you to move to this new API going forward. [Learn more](../privileged-identity-management/pim-apis.md).
 
---

### Revision of roles in Azure AD entitlement management

**Type:** Changed feature  
**Service category:** Roles  
**Product capability:** Entitlement Management
 
A new role Identity Governance Administrator has recently been introduced.This role will be the replacement for the User Administrator role in managing catalogs and access packages in Azure AD entitlement management. If you have assigned administrators to the User Administrator role or have them activate this role to manage access packages in Azure AD entitlement management, please switch to the Identity Governance Administrator role instead. The User Administrator role will no longer be providing administrative rights to catalogs or access packages. [Learn more](../governance/identity-governance-overview.md#appendix---least-privileged-roles-for-managing-in-identity-governance-features).

---

## April 2021

### Bug fixed - Azure AD will no longer double-encode the state parameter in responses

**Type:** Fixed  
**Service category:** Authentications (Logins)  
**Product capability:** User Authentication
 
Azure AD has identified, tested, and released a fix for a bug in the `/authorize` response to a client application.  Azure AD was incorrectly URL encoding the `state` parameter twice when sending responses back to the client.  This can cause a client application to reject the request, due to a mismatch in state parameters. [Learn more](../develop/reference-breaking-changes.md#bug-fix-azure-ad-will-no-longer-url-encode-the-state-parameter-twice). 

---

### Users can only create security and Microsoft 365 groups in Azure portal being deprecated

**Type:** Plan for change  
**Service category:** Group Management  
**Product capability:** Directory
 
Users will no longer be limited to create security and Microsoft 365 groups only in the Azure portal. The new setting will allow users to create security groups in the Azure portal, PowerShell, and API. Users will be required to verify and update the new setting. [Learn more](../enterprise-users/groups-self-service-management.md).

---

### Public Preview - External Identities Self-Service Sign-up in AAD using Email One-Time Passcode accounts

**Type:** New feature  
**Service category:** B2B  
**Product capability:** B2B/B2C
 
External users can now use Email One-Time Passcode accounts to sign up or sign in to Azure AD 1st party and line-of-business applications. [Learn more](../external-identities/one-time-passcode.md).

---

### General Availability - External Identities Self-Service Sign Up

**Type:** New feature  
**Service category:** B2B  
**Product capability:** B2B/B2C
 
Self-service sign-up for external users is now in general availability. With this new feature, external users can now self-service sign up to an application. 

You can create customized experiences for these external users, including collecting information about your users during the registration process and allowing external identity providers like Facebook and Google. You can also integrate with third-party cloud providers for various functionalities like identity verification or approval of users. [Learn more](../external-identities/self-service-sign-up-overview.md).
 
---

### General availability - Azure AD B2C Phone Sign-up and Sign-in using Built-in Policy

**Type:** New feature  
**Service category:** B2C - Consumer Identity Management  
**Product capability:** B2B/B2C
 
B2C Phone Sign-up and Sign-in using a built-in policy enable IT administrators and developers of organizations to allow their end-users to sign in and sign-up using a phone number in user flows. With this feature, disclaimer links such as privacy policy and terms of use can be customized and shown on the page before the end-user proceeds to receive the one-time passcode via text message. [Learn more](../../active-directory-b2c/phone-authentication-user-flows.md).
 
---

### New Federated Apps available in Azure AD Application gallery - April 2021

**Type:** New feature  
**Service category:** Enterprise Apps  
**Product capability:** 3rd Party Integration

In April 2021, we have added following 31 new applications in our App gallery with Federation support

[Zii Travel Azure AD Connect](http://ziitravel.com/), [Cerby](../saas-apps/cerby-tutorial.md), [Selflessly](https://app.selflessly.io/sign-in), [Apollo CX](https://apollo.cxlabs.de/sso/aad), [Pedagoo](https://account.pedagoo.com/), [Measureup](https://account.measureup.com/), [Wistec Education](https://wisteceducation.fi/login/index.php), [ProcessUnity](../saas-apps/processunity-tutorial.md), [Cisco Intersight](../saas-apps/cisco-intersight-tutorial.md), [Codility](../saas-apps/codility-tutorial.md), [H5mag](https://account.h5mag.com/auth/request-access/ms365), [Check Point Identity Awareness](../saas-apps/check-point-identity-awareness-tutorial.md), [Jarvis](https://jarvis.live/login), [desknet's NEO](../saas-apps/desknets-neo-tutorial.md), [SDS & Chemical Information Management](../saas-apps/sds-chemical-information-management-tutorial.md), [Wúru App](../saas-apps/wuru-app-tutorial.md), [Holmes](../saas-apps/holmes-tutorial.md), [Tide Multi Tenant](https://gallery.tideapp.co.uk/), [Telenor](https://admin.smartansatt.telenor.no/), [Yooz US](https://us1.getyooz.com/?kc_idp_hint=microsoft), [Mooncamp](https://app.mooncamp.com/#/login), [inwise SSO](https://app.inwise.com/defaultsso.aspx), [Ecolab Digital Solutions](https://ecolabb2c.b2clogin.com/account.ecolab.com/oauth2/v2.0/authorize?p=B2C_1A_Connect_OIDC_SignIn&client_id=01281626-dbed-4405-a430-66457825d361&nonce=defaultNonce&redirect_uri=https://jwt.ms&scope=openid&response_type=id_token&prompt=login), [Taguchi Digital Marketing System](https://login.taguchi.com.au/), [XpressDox EU Cloud](https://test.xpressdox.com/Authentication/Login.aspx), [EZSSH](https://docs.keytos.io/getting-started/registering-a-new-tenant/registering_app_in_tenant/), [EZSSH Client](https://portal.ezssh.io/signup), [Verto 365](https://www.vertocloud.com/Login/), [KPN Grip](https://www.grip-on-it.com/), [AddressLook](https://portal.bbsonlineservices.net/Manage/AddressLook), [Cornerstone Single Sign-On](../saas-apps/cornerstone-ondemand-tutorial.md)

You can also find the documentation of all the applications here: https://aka.ms/AppsTutorial

For listing your application in the Azure AD app gallery, read the details here: https://aka.ms/AzureADAppRequest

---

### New provisioning connectors in the Azure AD Application Gallery - April 2021

**Type:** New feature  
**Service category:** App Provisioning  
**Product capability:** 3rd Party Integration
 
You can now automate creating, updating, and deleting user accounts for these newly integrated apps:

- [Bentley - Automatic User Provisioning](../saas-apps/bentley-automatic-user-provisioning-tutorial.md)
- [Boxcryptor](../saas-apps/boxcryptor-provisioning-tutorial.md)
- [BrowserStack Single Sign-on](../saas-apps/browserstack-single-sign-on-provisioning-tutorial.md)
- [Eletive](../saas-apps/eletive-provisioning-tutorial.md)
- [Jostle](../saas-apps/jostle-provisioning-tutorial.md)
- [Olfeo SAAS](../saas-apps/olfeo-saas-provisioning-tutorial.md)
- [Proware](../saas-apps/proware-provisioning-tutorial.md)
- [Segment](../saas-apps/segment-provisioning-tutorial.md)

For more information about how to better secure your organization with automated user account provisioning, see [Automate user provisioning to SaaS applications with Azure AD](../app-provisioning/user-provisioning.md).
 
---

### Introducing new versions of page layouts for B2C

**Type:** Changed feature  
**Service category:** B2C - Consumer Identity Management  
**Product capability:** B2B/B2C
 
The [page layouts](../../active-directory-b2c/page-layout.md) for B2C scenarios on the Azure AD B2C has been updated to reduce security risks by introducing the new versions of jQuery and Handlebars JS.
 
---

### Updates to Sign-in Diagnostic

**Type:** Changed feature  
**Service category:** Reporting  
**Product capability:** Monitoring & Reporting
 
The scenario coverage of the Sign-in Diagnostic tool has increased. 

With this update, the following event-related scenarios will now be included in the sign-in diagnosis results: 
- Enterprise Applications configuration problem events.
- Enterprise Applications service provider (application-side) events.
- Incorrect credentials events. 

These results will show contextual and relevant details about the event and actions to take to resolve these problems. Also, for scenarios where we don't have deep contextual diagnostics, Sign-in Diagnostic will present more descriptive content about the error event.

For more information, see [What is sign-in diagnostic in Azure AD?](../reports-monitoring/overview-sign-in-diagnostics.md)

---
### Azure AD Connect cloud sync general availability refresh 
**Type:** Changed feature  
**Service category:** Azure AD Connect Cloud Sync 
**Product capability:** Directory

Azure AD connect cloud sync now has an updated agent (version# - 1.1.359). For more details on agent updates, including bug fixes, check out the [version history](../cloud-sync/reference-version-history.md). With the updated agent, cloud sync customers can use GMSA cmdlets to set and reset their gMSA permission at a granular level. In addition that, we have changed the limit of syncing members using group scope filtering from 1499 to 50,000 (50K) members. 

Check out the newly available [expression builder](../cloud-sync/how-to-expression-builder.md#deploy-the-expression) for cloud sync, which, helps you build complex expressions as well as simple expressions when you do transformations of attribute values from AD to Azure AD using attribute mapping.

---

## March 2021

### Guidance on how to enable support for TLS 1.2 in your environment, in preparation for upcoming Azure AD TLS 1.0/1.1 deprecation

**Type:** Plan for change  
**Service category:** N/A  
**Product capability:** Standards

Azure Active Directory will deprecate the following protocols in Azure Active Directory worldwide regions starting June 30, 2021:


- TLS 1.0
- TLS 1.1
- 3DES cipher suite (TLS_RSA_WITH_3DES_EDE_CBC_SHA)

Affected environments include:

- Azure Commercial Cloud
- Office 365 GCC and WW

For more information, see [Enable support for TLS 1.2 in your environment for Azure AD TLS 1.1 and 1.0 deprecation](/troubleshoot/azure/active-directory/enable-support-tls-environment).

---

### Public Preview - Azure AD Entitlement management now supports multi-geo SharePoint Online

**Type:** New feature  
**Service category:** Other  
**Product capability:** Entitlement Management
 
For organizations using multi-geo SharePoint Online, you can now include sites from specific multi-geo environments to your Entitlement management access packages. [Learn more](../governance/entitlement-management-catalog-create.md#add-a-multi-geo-sharepoint-site-preview).

---

### Public Preview - Restore deleted apps from App registrations

**Type:** New feature  
**Service category:** Other  
**Product capability:** Developer Experience
 
Customers can now view, restore, and permanently remove deleted app registrations from the Azure portal. This applies only to applications associated to a directory, not applications from a personal Microsoft account. [Learn more](../develop/howto-restore-app.md).
 
---

### Public preview - New "User action" in Conditional Access for registering or joining devices

**Type:** New feature  
**Service category:** Conditional Access  
**Product capability:** Identity Security & Protection
 
 A new user action called "Register or join devices" in Conditional access is available. This user action allows you to control Multi-factor authentication (MFA) policies for Azure AD device registration. 

Currently, this user action only allows you to enable MFA as a control when users register or join devices to Azure AD. Other controls that are dependent on or not applicable to Azure AD device registration are disabled with this user action. [Learn more](../conditional-access/concept-conditional-access-cloud-apps.md#user-actions). 
 
---

### Public Preview - Optimize connector groups to use the closest Application Proxy cloud service

**Type:** New feature  
**Service category:** App Proxy  
**Product capability:** Access Control
 
With this new capability, connector groups can be assigned to the closest regional Application Proxy service an application is hosted in. This can improve app performance in scenarios where apps are hosted in regions other than the home tenant’s region. [Learn more](../app-proxy/application-proxy-network-topology.md#optimize-connector-groups-to-use-closest-application-proxy-cloud-service-preview). 
 
---

### Public Preview - External Identities Self-Service Sign-up in AAD using Email One-Time Passcode accounts

**Type:** New feature  
**Service category:** B2B  
**Product capability:** B2B/B2C

External users will now be able to use Email One-Time Passcode accounts to sign up in to Azure AD 1st party and LOB apps. [Learn more](../external-identities/one-time-passcode.md).

---

### Public Preview - Availability of AD FS Sign-Ins in Azure AD

**Type:** New feature  
**Service category:** Authentications (Logins)  
**Product capability:** Monitoring & Reporting
 
AD FS sign-in activity can now be integrated with Azure AD activity reporting, providing a unified view of hybrid identity infrastructure. Using the Azure AD Sign-Ins report, Log Analytics, and Azure Monitor Workbooks, it's possible to do in-depth analysis for both AAD and AD FS sign-in scenarios such as AD FS account lockouts, bad password attempts, and spikes of unexpected sign-in attempts.

To learn more, visit [AD FS sign-ins in Azure AD with Connect Health](../hybrid/how-to-connect-health-ad-fs-sign-in.md).

---

### General availability - Staged rollout to cloud authentication

**Type:** New feature  
**Service category:** AD Connect  
**Product capability:** User Authentication
 
Staged rollout to cloud authentication is now generally available. The staged rollout feature allows you to selectively test groups of users with cloud authentication methods, such as Passthrough Authentication (PTA) or Password Hash Sync (PHS). Meanwhile, all other users in the federated domains continue to use federation services, such as AD FS or any other federation services to authenticate users. [Learn more](../hybrid/how-to-connect-staged-rollout.md).

---

### General Availability - User Type attribute can now be updated in the Azure admin portal

**Type:** New feature  
**Service category:** User Experience and Management  
**Product capability:** User Management
 
Customers can now update the user type of Azure AD users when they update their user profile information from the Azure admin portal. The user type can be updated from Microsoft Graph also. To learn more, see [Add or update user profile information](active-directory-users-profile-azure-portal.md).
 
---

### General Availability - Replica Sets for Azure Active Directory Domain Services

**Type:** New feature  
**Service category:** Azure AD Domain Services  
**Product capability:** Azure AD Domain Services
 
The capability of replica sets in Azure AD DS is now generally available. [Learn more](../../active-directory-domain-services/concepts-replica-sets.md).
 
---

### General availability - Collaborate with your partners using Email One-Time Passcode in the Azure Government cloud

**Type:** New feature  
**Service category:** B2B  
**Product capability:** B2B/B2C
 
Organizations in the Microsoft Azure Government cloud can now enable their guests to redeem invitations with Email One-Time Passcode. This ensures that any guest users with no Azure AD, Microsoft, or Gmail accounts in the Azure Government cloud can still collaborate with their partners by requesting and entering a temporary code to sign in to shared resources. [Learn more](../external-identities/one-time-passcode.md#note-for-azure-us-government-customers).

---

### New Federated Apps available in Azure AD Application gallery - March 2021

**Type:** New feature  
**Service category:** Enterprise Apps  
**Product capability:** 3rd Party Integration
 
In March 2021 we have added following 37 new applications in our App gallery with Federation support:

[Bambuser Live Video Shopping](https://lcx.bambuser.com/), [DeepDyve Inc](https://www.deepdyve.com/azure-sso), [Moqups](../saas-apps/moqups-tutorial.md), [RICOH Spaces Mobile](https://ricohspaces.app/welcome), [Flipgrid](https://auth.flipgrid.com/), [hCaptcha Enterprise](../saas-apps/hcaptcha-enterprise-tutorial.md), [SchoolStream ASA](https://jsd.schoolstreamk12.com/ASA/ASAlogin.aspx), [TransPerfect GlobalLink Dashboard](../saas-apps/transperfect-globallink-dashboard-tutorial.md), [SimplificaCI](https://app.simplificaci.com.br/), [Thrive LXP](../saas-apps/thrive-lxp-tutorial.md), [Lexonis TalentScape](../saas-apps/lexonis-talentscape-tutorial.md), [Exium](../saas-apps/exium-tutorial.md), [Sapient](../saas-apps/sapient-tutorial.md), [TrueChoice](../saas-apps/truechoice-tutorial.md), [RICOH Spaces](https://ricohspaces.app/welcome), [Saba Cloud](../saas-apps/learning-at-work-tutorial.md), [Acunetix 360](../saas-apps/acunetix-360-tutorial.md), [Exceed.ai](../saas-apps/exceed-ai-tutorial.md), [GitHub Enterprise Managed User](../saas-apps/github-enterprise-managed-user-tutorial.md), [Enterprise Vault.cloud for Outlook](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?response_type=id_token&scope=openid%20profile%20User.Read&client_id=7176efe5-e954-4aed-b5c8-f5c85a980d3a&nonce=4b9e1981-1bcb-4938-a283-86f6931dc8cb), [Smartlook](../saas-apps/smartlook-tutorial.md), [Accenture Academy](../saas-apps/accenture-academy-tutorial.md), [Onshape](../saas-apps/onshape-tutorial.md), [Tradeshift](../saas-apps/tradeshift-tutorial.md), [JuriBlox](../saas-apps/juriblox-tutorial.md), [SecurityStudio](../saas-apps/securitystudio-tutorial.md), [ClicData](https://app.clicdata.com/), [Evergreen](../saas-apps/evergreen-tutorial.md), [Patchdeck](https://patchdeck.com/ad_auth/authenticate/), [FAX.PLUS](../saas-apps/fax-plus-tutorial.md), [ValidSign](../saas-apps/validsign-tutorial.md), [AWS Single Sign-on](../saas-apps/aws-single-sign-on-tutorial.md), [Nura Space](https://dashboard.nuraspace.com/login), [Broadcom DX SaaS](../saas-apps/broadcom-dx-saas-tutorial.md), [Interplay Learning](https://skilledtrades.interplaylearning.com/#login), [SendPro Enterprise](../saas-apps/sendpro-enterprise-tutorial.md), [FortiSASE SIA](../saas-apps/fortisase-sia-tutorial.md)

You can also find the documentation of all the applications here: https://aka.ms/AppsTutorial

For listing your application in the Azure AD app gallery, read the details here: https://aka.ms/AzureADAppRequest

---

### New provisioning connectors in the Azure AD Application Gallery - March 2021

**Type:** New feature  
**Service category:** App Provisioning  
**Product capability:** 3rd Party Integration

You can now automate creating, updating, and deleting user accounts for these newly integrated apps:

- [AWS Single Sign-on](../saas-apps/aws-single-sign-on-provisioning-tutorial.md)
- [Bpanda](../saas-apps/bpanda-provisioning-tutorial.md)
- [Britive](../saas-apps/britive-provisioning-tutorial.md)
- [GitHub Enterprise Managed User](../saas-apps/github-enterprise-managed-user-provisioning-tutorial.md)
- [Grammarly](../saas-apps/grammarly-provisioning-tutorial.md)
- [LogicGate](../saas-apps/logicgate-provisioning-tutorial.md)
- [SecureLogin](../saas-apps/secure-login-provisioning-tutorial.md)
- [TravelPerk](../saas-apps/travelperk-provisioning-tutorial.md)

For more information about how to better secure your organization by using automated user account provisioning, see [Automate user provisioning to SaaS applications with Azure AD](../app-provisioning/user-provisioning.md).
 
---

### Introducing MS Graph API for Company Branding

**Type:** Changed feature  
**Service category:** MS Graph  
**Product capability:** B2B/B2C

[MS Graph API for the Company Branding](/graph/api/resources/organizationalbrandingproperties)  is available for the Azure AD or Microsoft 365 login experience to allow the management of the branding parameters programmatically.

---

### General availability - Header-based authentication SSO with Application Proxy

**Type:** Changed feature  
**Service category:** App Proxy  
**Product capability:** Access Control
 
Azure AD Application Proxy native support for header-based authentication is now in general availability. With this feature, you can configure the user attributes required as HTTP headers for the application without additional components needed to deploy. [Learn more](../app-proxy/application-proxy-configure-single-sign-on-with-headers.md).

---

### Two-way SMS for MFA Server is no longer supported

**Type:** Deprecated  
**Service category:** MFA  
**Product capability:** Identity Security & Protection
 

Two-way SMS for MFA Server was originally deprecated in 2018, and will not be supported after February 24, 2021. Administrators should enable another method for users who still use two-way SMS.

Email notifications and Azure portal Service Health notifications were sent to affected admins on December 8, 2020 and January 28, 2021. The alerts went to the Owner, Co-Owner, Admin, and Service Admin RBAC roles tied to the subscriptions. [Learn more](../authentication/how-to-authentication-two-way-sms-unsupported.md).
 
---
 
## February 2021

### Email one-time passcode authentication on by default starting October 2021

**Type:** Plan for change  
**Service category:** B2B  
**Product capability:** B2B/B2C
 

Starting October 31, 2021, Microsoft Azure Active Directory [email one-time passcode authentication](../external-identities/one-time-passcode.md) will become the default method for inviting accounts and tenants for B2B collaboration scenarios. At this time, Microsoft will no longer allow the redemption of invitations using unmanaged Azure Active Directory accounts. 

---

### Unrequested but consented permissions will no longer be added to tokens if they would trigger Conditional Access

**Type:** Plan for change  
**Service category:** Authentications (Logins)  
**Product capability:** Platform
 
Currently, applications using [dynamic permissions](../develop/v2-permissions-and-consent.md#requesting-individual-user-consent) are given all of the permissions they're consented to access. This includes applications that are unrequested and even if they trigger conditional access. For example, this can cause an app requesting only `user.read` that also has consent for `files.read`, to be forced to pass the Conditional Access assigned for the `files.read` permission. 

To reduce the number of unnecessary Conditional Access prompts, Azure AD is changing the way that unrequested scopes are provided to applications. Apps will only trigger conditional access for permission they explicitly request. For more information, read [What's new in authentication](../develop/reference-breaking-changes.md#conditional-access-will-only-trigger-for-explicitly-requested-scopes).
 
---
 
### Public Preview - Use a Temporary Access Pass to register Passwordless credentials

**Type:** New feature  
**Service category:** MFA  
**Product capability:** Identity Security & Protection

Temporary Access Pass is a time-limited passcode that serves as strong credentials and allows onboarding of Passwordless credentials and recovery when a user has lost or forgotten their strong authentication factor (for example, FIDO2 security key or Microsoft Authenticator) app and needs to sign in to register new strong authentication methods. [Learn more](../authentication/howto-authentication-temporary-access-pass.md).

---

### Public preview - Keep me signed in (KMSI) in next generation of user flows

**Type:** New feature  
**Service category:** B2C - Consumer Identity Management  
**Product capability:** B2B/B2C

The next generation of B2C user flows now supports the [keep me signed in (KMSI)](../../active-directory-b2c/session-behavior.md?pivots=b2c-custom-policy#enable-keep-me-signed-in-kmsi) functionality that allows customers to extend the session lifetime for the users of their web and native applications by using a persistent cookie.  feature keeps the session active even when the user closes and reopens the browser, and is revoked when the user signs out.

---

### Public preview - External Identities Self-Service Sign-up in AAD using MSA accounts

**Type:** New feature  
**Service category:** B2B  
**Product capability:** B2B/B2C
 
External users will can now use Microsoft Accounts to sign in to Azure AD first party and LOB apps. [Learn more](../external-identities/self-service-sign-up-overview.md).

---

### Public Preview - Reset redemption status for a guest user

**Type:** New feature  
**Service category:** B2B  
**Product capability:** B2B/B2C
 
Customers can now reinvite existing external guest users to reset their redemption status, which allows the guest user account to remain without them losing any access. [Learn more](../external-identities/reset-redemption-status.md).
 
---

### Public Preview - /synchronization (provisioning) APIs now support application permissions

**Type:** New feature  
**Service category:** App Provisioning  
**Product capability:** Identity Lifecycle Management
 
Customers can now use application.readwrite.ownedby as an application permission to call the synchronization APIs. Note this is only supported for provisioning from Azure AD out into third-party applications (for example, AWS, Data Bricks, etc.). It is currently not supported for HR-provisioning (Workday / Successfactors) or Cloud Sync (AD to Azure AD). [Learn more](/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta&preserve-view=true).
 
---

### General Availability - Authentication Policy Administrator built-in role

**Type:** New feature  
**Service category:** RBAC  
**Product capability:** Access Control
 
Users with this role can configure the authentication methods policy, tenant-wide MFA settings, and password protection policy. This role grants permission to manage Password Protection settings: smart lockout configurations and updating the custom banned passwords list. [Learn more](../roles/permissions-reference.md#authentication-policy-administrator).

---

### General availability - User collections on My Apps are available now!

**Type:** New feature  
**Service category:** My Apps  
**Product capability:** End User Experiences
 
Users can now create their own groupings of apps on the My Apps app launcher. They can also reorder and hide collections shared with them by their administrator. [Learn more](../user-help/my-apps-portal-user-collections.md).

---

### General availability - Autofill in Authenticator

**Type:** New feature  
**Service category:** Microsoft Authenticator App  
**Product capability:** Identity Security & Protection
 
Microsoft Authenticator provides multi-factor authentication (MFA) and account management capabilities, and now also will autofill passwords on sites and apps users visit on their mobile (iOS and Android). 

To use autofill on Authenticator, users need to add their personal Microsoft account to Authenticator and use it to sync their passwords. Work or school accounts cannot be used to sync passwords at this time. [Learn more](../user-help/user-help-auth-app-faq.md#autofill-for-it-admins).

---

### General availability - Invite internal users to B2B collaboration

**Type:** New feature  
**Service category:** B2B  
**Product capability:** B2B/B2C
 
Customers can now invite internal guests to use B2B collaboration instead of sending an invitation to an existing internal account. This allows customers to keep that user's object ID, UPN, group memberships, and app assignments. [Learn more](../external-identities/invite-internal-users.md).

---

### General Availability - Domain Name Administrator built-in role

**Type:** New feature  
**Service category:** RBAC  
**Product capability:** Access Control
 
Users with this role can manage (read, add, verify, update, and delete) domain names. They can also read directory information about users, groups, and applications, as these objects have domain dependencies. 

For on-premises environments, users with this role can configure domain names for federation so that associated users are always authenticated on-premises. These users can then sign into Azure AD-based services with their on-premises passwords via single sign-on. Federation settings need to be synced via Azure AD Connect, so users also have permissions to manage Azure AD Connect. [Learn more](../roles/permissions-reference.md#domain-name-administrator).
 
---

### New Federated Apps available in Azure AD Application gallery - February 2021

**Type:** New feature  
**Service category:** Enterprise Apps  
**Product capability:** 3rd Party Integration
 
In February 2021 we have added following 37 new applications in our App gallery with Federation support:

[Loop Messenger Extension](https://loopworks.com/loop-flow-messenger/), [Silverfort Azure AD Adapter](http://www.silverfort.com/), [Interplay Learning](https://skilledtrades.interplaylearning.com/#login), [Nura Space](https://dashboard.nuraspace.com/login), [Yooz EU](https://eu1.getyooz.com/?kc_idp_hint=microsoft), [UXPressia](https://uxpressia.com/users/sign-in), [introDus Pre- and Onboarding Platform](http://app.introdus.dk/login), [Happybot](https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize?client_id=34353e1e-dfe5-4d2f-bb09-2a5e376270c8&response_type=code&redirect_uri=https://api.happyteams.io/microsoft/integrate&response_mode=query&scope=offline_access%20User.Read%20User.Read.All), [LeaksID](https://app.leaksid.com/), [ShiftWizard](http://www.shiftwizard.com/), [PingFlow SSO](https://app.pingview.io/), [Swiftlane](https://admin.swiftlane.com/login), [Quasydoc SSO](https://www.quasydoc.eu/login), [Fenwick Gold Account](https://businesscentral.dynamics.com/), [SeamlessDesk](https://www.seamlessdesk.com/login), [Learnsoft LMS & TMS](http://www.learnsoft.com/), [P-TH+](https://p-th.jp/), [myViewBoard](https://api.myviewboard.com/auth/microsoft/), [Tartabit IoT Bridge](https://bridge-us.tartabit.com/), [AKASHI](../saas-apps/akashi-tutorial.md), [Rewatch](../saas-apps/rewatch-tutorial.md), [Zuddl](../saas-apps/zuddl-tutorial.md), [Parkalot - Car park management](../saas-apps/parkalot-car-park-management-tutorial.md), [HSB ThoughtSpot](../saas-apps/hsb-thoughtspot-tutorial.md), [IBMid](../saas-apps/ibmid-tutorial.md), [SharingCloud](../saas-apps/sharingcloud-tutorial.md), [PoolParty Semantic Suite](../saas-apps/poolparty-semantic-suite-tutorial.md), [GlobeSmart](../saas-apps/globesmart-tutorial.md), [Samsung Knox and Business Services](../saas-apps/samsung-knox-and-business-services-tutorial.md), [Penji](../saas-apps/penji-tutorial.md), [Kendis- Scaling Agile Platform](../saas-apps/kendis-scaling-agile-platform-tutorial.md), [Maptician](../saas-apps/maptician-tutorial.md), [Olfeo SAAS](../saas-apps/olfeo-saas-tutorial.md), [Sigma Computing](../saas-apps/sigma-computing-tutorial.md), [CloudKnox Permissions Management Platform](../saas-apps/cloudknox-permissions-management-platform-tutorial.md), [Klaxoon SAML](../saas-apps/klaxoon-saml-tutorial.md), [Enablon](../saas-apps/enablon-tutorial.md)

You can also find the documentation of all the applications here: https://aka.ms/AppsTutorial

For listing your application in the Azure AD app gallery, read the details here: https://aka.ms/AzureADAppRequest

--- 

### New provisioning connectors in the Azure AD Application Gallery - February 2021

**Type:** New feature  
**Service category:** App Provisioning  
**Product capability:** 3rd Party Integration
 

You can now automate creating, updating, and deleting user accounts for these newly integrated apps:

- [Atea](../saas-apps/atea-provisioning-tutorial.md)
- [Getabstract](../saas-apps/getabstract-provisioning-tutorial.md)
- [HelloID](../saas-apps/helloid-provisioning-tutorial.md)
- [Hoxhunt](../saas-apps/hoxhunt-provisioning-tutorial.md)
- [Iris Intranet](../saas-apps/iris-intranet-provisioning-tutorial.md)
- [Preciate](../saas-apps/preciate-provisioning-tutorial.md)

For more information, read [Automate user provisioning to SaaS applications with Azure AD](../app-provisioning/user-provisioning.md).

---

### General Availability - 10 Azure Active Directory roles now renamed

**Type:** Changed feature  
**Service category:** RBAC  
**Product capability:** Access Control
 
10 Azure AD built-in roles have been renamed so that they're aligned across the [Microsoft 365 admin center](/microsoft-365/admin/microsoft-365-admin-center-preview), [Azure AD portal](https://portal.azure.com/), and [Microsoft Graph](https://developer.microsoft.com/graph/). To learn more about the new roles, refer to [Administrator role permissions in Azure Active Directory](../roles/permissions-reference.md#all-roles).

![Table showing role names in MS Graph API and the Azure portal, and the proposed final name across API, Azure portal, and Mac.](media/whats-new/roles-table-rbac.png)

---

### New Company Branding in MFA/SSPR Combined Registration

**Type:** Changed feature  
**Service category:** User Experience and Management  
**Product capability:** End User Experiences
 
In the past, company logos weren't used on Azure Active Directory sign-in pages. Company branding is now located to the top left of MFA/SSPR Combined Registration. Company branding is also included on My Sign-Ins and the Security Info page. [Learn more](../fundamentals/customize-branding.md).

---

### General Availability - Second level manager can be set as alternate approver

**Type:** Changed feature  
**Service category:** User Access Management  
**Product capability:** Entitlement Management
 
An extra option when you select approvers is now available in Entitlement Management. If you select "Manager as approver" for the First Approver, you will have another option, "Second level manager as alternate approver", available to choose in the alternate approver field. If you select this option, you need to add a fallback approver to forward the request to in case the system can't find the second level manager. [Learn more](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers).
 
---

### Authentication Methods Activity Dashboard

**Type:** Changed feature  
**Service category:** Reporting  
**Product capability:** Monitoring & Reporting
 

The refreshed Authentication Methods Activity dashboard gives admins an overview of authentication method registration and usage activity in their tenant. The report summarizes the number of users registered for each method, and also which methods are used during sign-in and password reset. [Learn more](../authentication/howto-authentication-methods-activity.md).
 
---

### Refresh and session token lifetimes configurability in Configurable Token Lifetime (CTL) are retired

**Type:** Deprecated  
**Service category:** Other  
**Product capability:** User Authentication
 
Refresh and session token lifetimes configurability in CTL are retired. Azure Active Directory no longer honors refresh and session token configuration in existing policies. [Learn more](../develop/active-directory-configurable-token-lifetimes.md#token-lifetime-policies-for-refresh-tokens-and-session-tokens).
 
---
 
## January 2021

### Secret token will be a mandatory field when configuring provisioning

**Type:** Plan for change  
**Service category:** App Provisioning  
**Product capability:** Identity Lifecycle Management

In the past, the secret token field could be kept empty when setting up provisioning on the custom / BYOA application. This function was intended to solely be used for testing. We'll update the UI to make the field required. 

Customers can work around this requirement for testing purposes by using a feature flag in the browser URL. [Learn more](../app-provisioning/use-scim-to-provision-users-and-groups.md#authorization-to-provisioning-connectors-in-the-application-gallery).
 
---

### Public Preview - Customize and configure Android shared devices for frontline workers at scale

**Type:** New feature  
**Service category:** Device Registration and Management  
**Product capability:** Identity Security & Protection
 
Azure AD and Microsoft Endpoint Manager teams have combined to bring the capability to customize, scale, and secure your frontline worker devices.

The following preview capabilities will allow you to:
- Provision Android shared devices at scale with Microsoft Endpoint Manager
- Secure your access for shift workers using device-based conditional access
- Customize sign-in experiences for the shift workers with Managed Home Screen

To learn more, refer to [Customize and configure shared devices for frontline workers at scale](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/customize-and-configure-shared-devices-for-firstline-workers-at/ba-p/1751708).

---

### Public preview - Provisioning logs can now be downloaded as a CSV or JSON

**Type:** New feature  
**Service category:** App Provisioning  
**Product capability:** Identity Lifecycle Management

Customers can download the provisioning logs as a CSV or JSON file through the UI and via graph API. To learn more, refer to [Provisioning reports in the Azure Active Directory portal](../reports-monitoring/concept-provisioning-logs.md).

---

### Public preview - Assign cloud groups to Azure AD custom roles and admin unit scoped roles

**Type:** New feature  
**Service category:** RBAC  
**Product capability:** Access Control
 
Customers can assign a cloud group to Azure AD custom roles or an admin unit scoped role. To learn how to use this feature, refer to [Use cloud groups to manage role assignments in Azure Active Directory](../roles/groups-concept.md).

---

### General Availability - Azure AD Connect cloud sync (previously known as cloud provisioning)

**Type:** New feature  
**Service category:** Azure AD Connect cloud sync  
**Product capability:** Identity Lifecycle Management
 
Azure AD Connect cloud sync is now generally available to all customers.

Azure AD Connect cloud moves the heavy lifting of transform logic to the cloud, reducing your on-premises footprint. Additionally, multiple light-weight agent deployments are available for higher sync availability. [Learn more](https://aka.ms/cloudsyncGA).
 
---
### General Availability - Attack Simulation Administrator and Attack Payload Author built-in roles

**Type:** New feature  
**Service category:** RBAC  
**Product capability:** Access Control
 
Two new roles in Role-Based Access Control are available to assign to users, Attack simulation Administrator and Attack Payload author. 

Users in the [Attack Simulation Administrator](../roles/permissions-reference.md#attack-simulation-administrator) role have access for all simulations in the tenant and can:
- create and manage all aspects of attack simulation creation
- launch/scheduling of a simulation
-  review simulation results. 

Users in the [Attack Payload Author](../roles/permissions-reference.md#attack-payload-author) role can create attack payloads but not actually launch or schedule them. Attack payloads are then available to all administrators in the tenant who can use them to create a simulation.

---

### General Availability - Usage Summary Reports Reader built-in role

**Type:** New feature  
**Service category:** RBAC  
**Product capability:** Access Control
 
Users with the Usage Summary Reports Reader role can access tenant level aggregated data and associated insights in Microsoft 365 Admin Center for Usage and Productivity Score. However, they can't access any user level details or insights. 

In the Microsoft 365 Admin Center for the two reports, we differentiate between tenant level aggregated data and user level details. This role adds an extra layer of protection to individual user identifiable data. [Learn more](../roles/permissions-reference.md#usage-summary-reports-reader).

---

### General availability - Require App protection policy grant in Azure AD Conditional Access

**Type:** New Feature  
**Service category:** Conditional Access  
**Product capability:** Identity Security & Protection
 
Azure AD Conditional Access grant for "Require App Protection policy" is now GA. 

The policy provides the following capabilities:
- Allows access only when using a mobile application that supports Intune App protection
- Allows access only when a user has an Intune app protection policy delivered to the mobile application

Learn more on how to set up a conditional access policy for app protection [here](../conditional-access/app-protection-based-conditional-access.md).
 
---

### General availability - Email One-Time Passcode

**Type:** New feature  
**Service category:** B2B  
**Product capability:** B2B/B2C
 
Email OTP enables organizations around the world to collaborate with anyone by sending a link or invitation via email. Invited users can verify their identity with the one-time passcode sent to their email to access their partner's resources. [Learn more](../external-identities/one-time-passcode.md). 
 
---

 ### New provisioning connectors in the Azure AD Application Gallery - January 2021

**Type:** New feature  
**Service category:** App Provisioning  
**Product capability:** 3rd Party Integration
 
You can now automate creating, updating, and deleting user accounts for these newly integrated apps:
- [Fortes Change Cloud](../saas-apps/fortes-change-cloud-provisioning-tutorial.md)
- [Gtmhub](../saas-apps/gtmhub-provisioning-tutorial.md)
- [monday.com](../saas-apps/mondaycom-provisioning-tutorial.md)
- [Splashtop](../saas-apps/splashtop-provisioning-tutorial.md)
- [Templafy OpenID Connect](../saas-apps/templafy-openid-connect-provisioning-tutorial.md)
- [WEDO](../saas-apps/wedo-provisioning-tutorial.md)

For more information, see [What is automated SaaS app user provisioning in Azure AD?](../app-provisioning/user-provisioning.md)

---

### New Federated Apps available in Azure AD Application gallery - January 2021

**Type:** New feature  
**Service category:** Enterprise Apps  
**Product capability:** 3rd Party Integration

In January 2021 we have added following 29 new applications in our App gallery with Federation support:

[mySCView](https://dev.myscview.com/), [Talentech](https://talentech.com/contact/), [Bipsync](https://www.bipsync.com/), [OroTimesheet](https://app.orotimesheet.com/login.php), [Mio](https://app.m.io/auth/install/microsoft?scopetype=hub), [Sovelto Easy](https://login.soveltoeasy.fi/), [Supportbench](https://account.supportbench.net/agent/login/),[Bienvenue Formation](https://formation.bienvenue.pro/login), [AIDA Healthcare SSO](https://aidaforparents.com/login/organizations), [International SOS Assistance Products](../saas-apps/international-sos-assistance-products-tutorial.md), [NAVEX One](../saas-apps/navex-one-tutorial.md), [LabLog](../saas-apps/lablog-tutorial.md), [Oktopost SAML](../saas-apps/oktopost-saml-tutorial.md), [EPHOTO DAM](../saas-apps/ephoto-dam-tutorial.md), [Notion](../saas-apps/notion-tutorial.md), [Syndio](../saas-apps/syndio-tutorial.md), [Yello Enterprise](../saas-apps/yello-enterprise-tutorial.md), [Timeclock 365 SAML](../saas-apps/timeclock-365-saml-tutorial.md), [Nalco E-data](https://www.ecolab.com/), [Vacancy Filler](https://app.vacancy-filler.co.uk/VFMVC/Account/Login), [Synerise AI Growth Ecosystem](../saas-apps/synerise-ai-growth-ecosystem-tutorial.md), [Imperva Data Security](../saas-apps/imperva-data-security-tutorial.md), [Illusive Networks](../saas-apps/illusive-networks-tutorial.md), [Proware](../saas-apps/proware-tutorial.md), [Splan Visitor](../saas-apps/splan-visitor-tutorial.md), [Aruba User Experience Insight](../saas-apps/aruba-user-experience-insight-tutorial.md), [Contentsquare SSO](../saas-apps/contentsquare-sso-tutorial.md), [Perimeter 81](../saas-apps/perimeter-81-tutorial.md), [Burp Suite Enterprise Edition](../saas-apps/burp-suite-enterprise-edition-tutorial.md)

You can also find the documentation of all the applications from here https://aka.ms/AppsTutorial

For listing your application in the Azure AD app gallery, read the details here https://aka.ms/AzureADAppRequest 

---

### Public preview - Second level manager can be set as alternate approver

**Type:** Changed feature  
**Service category:** User Access Management  
**Product capability:** Entitlement Management
 
An extra option when you select approvers is now available in Entitlement Management. If you select "Manager as approver" for the First Approver, you will have another option, "Second level manager as alternate approver", available to choose in the alternate approver field. If you select this option, you need to add a fallback approver to forward the request to in case the system can't find the second level manager. [Learn more](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers)
 
---

### General availability - Navigate to Teams directly from My Access portal

**Type:** Changed feature  
**Service category:** User Access Management  
**Product capability:** Entitlement Management
 
You can now launch Teams directly from the My Access portal. 

To do so, sign-in to My Access (https://myaccess.microsoft.com/), navigate to "Access packages", then go to the "Active" tab to see all of the access packages you already have access to. When you expand the selected access package and hover on Teams, you can launch it by clicking on the "Open" button. [Learn more](../governance/entitlement-management-request-access.md).
 
---

### Improved Logging & End-User Prompts for Risky Guest Users

**Type:** Changed feature  
**Service category:** Identity Protection  
**Product capability:** Identity Security & Protection
 

The Logging and End-User Prompts for Risky Guest Users have been updated. Learn more in [Identity Protection and B2B users](../identity-protection/concept-identity-protection-b2b.md).
 
---
 
## December 2020

### Public preview - Azure AD B2C Phone Sign-up and Sign-in using Built-in Policy

**Type:** New feature  
**Service category:** B2C - Consumer Identity Management  
**Product capability:** B2B/B2C
 
B2C Phone Sign-up and Sign-in using Built-in Policy enable IT administrators and developers of organizations to allow their end-users to sign in and sign up using a phone number in user flows. Read [Set up phone sign-up and sign-in for user flows (preview)](../../active-directory-b2c/phone-authentication-user-flows.md) to learn more.

---

### General Availability - Security Defaults now enabled for all new tenants by default

**Type:** New feature  
**Service category:** Other  
**Product capability:** Identity Security & Protection
 
To protect user accounts, all new tenants created on or after November 12, 2020, will come with Security Defaults enabled. Security Defaults enforces multiple policies including:
- Requires all users and admins to register for MFA using the Microsoft Authenticator App
- Requires critical admin roles to use MFA every single time they sign-in. All other users will be prompted for MFA whenever necessary. 
- Legacy authentication will be blocked tenant wide. 

For more information, read [What are security defaults?](../fundamentals/concept-fundamentals-security-defaults.md)

---

### General availability - Support for groups with up to 250K members in AADConnect

**Type:** Changed feature  
**Service category:** AD Connect  
**Product capability:** Identity Lifecycle Management
 
Microsoft has deployed a new endpoint (API) for Azure AD Connect that improves the performance of the synchronization service operations to Azure Active Directory. When you use the new [V2 endpoint](../hybrid/how-to-connect-sync-endpoint-api-v2.md), you'll experience noticeable performance gains on export and import to Azure AD. This new endpoint supports the following scenarios:

- Syncing groups with up to 250k members
- Performance gains on export and import to Azure AD

---

### General availability - Entitlement Management available for tenants in Azure China cloud

**Type:** New feature  
**Service category:** User Access Management  
**Product capability:** Entitlement Management
 

The capabilities of Entitlement Management are now available for all tenants in the Azure China cloud. For information, visit our [Identity governance documentation](https://docs.azure.cn/zh-cn/active-directory/governance/) site.

---

### New provisioning connectors in the Azure AD Application Gallery - December 2020

**Type:** New feature  
**Service category:** App Provisioning  
**Product capability:** 3rd Party Integration

You can now automate creating, updating, and deleting user accounts for these newly integrated apps:

- [Bizagi Studio for Digital Process Automation](../saas-apps/bizagi-studio-for-digital-process-automation-provisioning-tutorial.md)
- [CybSafe](../saas-apps/cybsafe-provisioning-tutorial.md)
- [GroupTalk](../saas-apps/grouptalk-provisioning-tutorial.md)
- [PaperCut Cloud Print Management](../saas-apps/papercut-cloud-print-management-provisioning-tutorial.md)
- [Parsable](../saas-apps/parsable-provisioning-tutorial.md)
- [Shopify Plus](../saas-apps/shopify-plus-provisioning-tutorial.md)

For more information about how to better secure your organization by using automated user account provisioning, see [Automate user provisioning to SaaS applications with Azure AD](../app-provisioning/user-provisioning.md).
 
---

### New Federated Apps available in Azure AD Application gallery - December 2020

**Type:** New feature  
**Service category:** Enterprise Apps  
**Product capability:** 3rd Party Integration
 
In December 2020 we have added following 18 new applications in our App gallery with Federation support:

[AwareGo](../saas-apps/awarego-tutorial.md), [HowNow SSO](https://gethownow.com/), [ZyLAB ONE Legal Hold](https://www.zylab.com/en/product/legal-hold), [Guider](http://www.guider-ai.com/), [Softcrisis](https://www.softcrisis.se/sv/), [Pims 365](http://www.omega365.com/pims), [InformaCast](../saas-apps/informacast-tutorial.md), [RetrieverMediaDatabase](../saas-apps/retrievermediadatabase-tutorial.md), [vonage](../saas-apps/vonage-tutorial.md), [Count Me In - Operations Dashboard](../saas-apps/count-me-in-operations-dashboard-tutorial.md), [ProProfs Knowledge Base](../saas-apps/proprofs-knowledge-base-tutorial.md), [RightCrowd Workforce Management](../saas-apps/rightcrowd-workforce-management-tutorial.md), [JLL TRIRIGA](../saas-apps/jll-tririga-tutorial.md), [Shutterstock](../saas-apps/shutterstock-tutorial.md), [FortiWeb Web Application Firewall](../saas-apps/linkedin-talent-solutions-tutorial.md), [LinkedIn Talent Solutions](../saas-apps/linkedin-talent-solutions-tutorial.md), [Equinix Federation App](../saas-apps/equinix-federation-app-tutorial.md), [KFAdvance](../saas-apps/kfadvance-tutorial.md)

You can also find the documentation of all the applications from here https://aka.ms/AppsTutorial

For listing your application in the Azure AD app gallery, read the details here https://aka.ms/AzureADAppRequest

---

### Navigate to Teams directly from My Access portal

**Type:** Changed feature  
**Service category:** User Access Management 
**Product capability:** Entitlement Management

You can now launch Teams directly from My Access portal. To do so, sign-in to [My Access](https://myaccess.microsoft.com/), navigate to **Access packages**, then go to the **Active** Tab to see all access packages you already have access to. When you expand the access package and hover on Teams, you can launch it by clicking on the **Open** button. 

To learn more about using the My Access portal, go to [Request access to an access package in Azure AD entitlement management](../governance/entitlement-management-request-access.md#sign-in-to-the-my-access-portal).

---

### Public preview - Second level manager can be set as alternate approver

**Type:** Changed feature  
**Service category:** User Access Management  
**Product capability:** Entitlement Management

An extra option is now available in the approval process in Entitlement Management. If you select Manager as approver for the First Approver, you'll have another option, Second level manager as alternate approver, available to choose in the alternate approver field. When you select this option, you need to add a fallback approver to forward the request to in case the system can't find the second level manager.

For more information, go to [Change approval settings for an access package in Azure AD entitlement management](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers).

---

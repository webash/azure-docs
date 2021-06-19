---
title: What is Privileged Identity Management? - Azure AD | Microsoft Docs
description: Provides an overview of Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 06/15/2021
ms.author: curtand
ms.custom: pim,azuread-video-2020,contperf-fy21q3-portal
ms.collection: M365-identity-device-management
---

# What is Azure AD Privileged Identity Management?

 Privileged Identity Management (PIM) is a service in Azure Active Directory (Azure AD) that enables you to manage, control, and monitor access to important resources in your organization. These resources include resources in Azure AD, Azure, and other Microsoft Online Services such as Microsoft 365 or Microsoft Intune. The following video introduces you to important PIM concepts and features.
<br><br>

> [!VIDEO https://www.youtube.com/embed/f-0K7mRUPpQ]

## Reasons to use

Organizations want to minimize the number of people who have access to secure information or resources, because that reduces the chance of

- a malicious actor getting access
- an authorized user inadvertently impacting a sensitive resource

However, users still need to carry out privileged operations in Azure AD, Azure, Microsoft 365, or SaaS apps. Organizations can give users just-in-time privileged access to Azure and Azure AD resources and can oversee what those users are doing with their privileged access.

## License requirements

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

For information about licenses for users, see [License requirements to use Privileged Identity Management](subscription-requirements.md).

## What does it do?

Privileged Identity Management provides time-based and approval-based role activation to mitigate the risks of excessive, unnecessary, or misused access permissions on resources that you care about. Here are some of the key features of Privileged Identity Management:

- Provide **just-in-time** privileged access to Azure AD and Azure resources
- Assign **time-bound** access to resources using start and end dates
- Require **approval** to activate privileged roles
- Enforce **multi-factor authentication** to activate any role
- Use **justification** to understand why users activate
- Get **notifications** when privileged roles are activated
- Conduct **access reviews** to ensure users still need roles
- Download **audit history** for internal or external audit

## What can I do with it?

Once you set up Privileged Identity Management, you'll see **Tasks**, **Manage**, and **Activity** options in the left navigation menu. As an administrator, you'll choose between options such as managing **Azure AD roles**, managing **Azure resource** roles, or privileged access groups. When you choose what you want to manage, you see the appropriate set of options for that option.

![Screenshot of Privileged Identity Management in the Azure portal](./media/pim-configure/pim-quickstart.png)

## Who can do what?

For Azure AD roles in Privileged Identity Management, only a user who is in the Privileged Role Administrator or Global Administrator role can manage assignments for other administrators. Global Administrators, Security Administrators, Global Readers, and Security Readers can also view assignments to Azure AD roles in Privileged Identity Management.

For Azure resource roles in Privileged Identity Management, only a subscription administrator, a resource Owner, or a resource User Access administrator can manage assignments for other administrators. Users who are Privileged Role Administrators, Security Administrators, or Security Readers do not by default have access to view assignments to Azure resource roles in Privileged Identity Management.

## Extend and renew assignments

After you set up your time-bound owner or member assignments, the first question you might ask is what happens if an assignment expires? In this new version, we provide two options for this scenario:

- Extend – When a role assignment nears expiration, the user can use Privileged Identity Management to request an extension for the role assignment
- Renew – When a role assignment has already expired, the user can use Privileged Identity Management to request a renewal for the role assignment

Both user-initiated actions require an approval from a Global Administrator or Privileged Role Administrator. Admins don't need to be in the business of managing assignment expirations. You can just wait for the extension or renewal requests to arrive for simple approval or denial.

## Scenarios

Privileged Identity Management supports the following scenarios:

### Privileged Role Administrator permissions

- Enable approval for specific roles
- Specify approver users or groups to approve requests
- View request and approval history for all privileged roles

### Approver permissions

- View pending approvals (requests)
- Approve or reject requests for role elevation (single and bulk)
- Provide justification for my approval or rejection

### Eligible role user permissions

- Request activation of a role that requires approval
- View the status of your request to activate
- Complete your task in Azure AD if activation was approved

## Managing privileged access Azure AD groups (preview)

In Privileged Identity Management (PIM), you can now assign eligibility for membership or ownership of privileged access groups. Starting with this preview, you can assign Azure Active Directory (Azure AD) built-in roles to cloud groups and use PIM to manage group member and owner eligibility and activation. For more information about role-assignable groups in Azure AD, see [Use cloud groups to manage role assignments in Azure Active Directory (preview)](../roles/groups-concept.md).

>[!Important]
> To assign a privileged access group to a role for administrative access to Exchange, Security and Compliance center, or SharePoint, use the Azure AD portal **Roles and Administrators** experience and not in the Privileged Access Groups experience to make the user or group eligible for activation into the group.

### Different just-in-time policies for each group

Some organizations use tools like Azure AD business-to-business (B2B) collaboration to invite their partners as guests to their Azure AD organization. Instead of a single just-in-time policy for all assignments to a privileged role, you can create two different privileged access groups with their own policies. You can enforce less strict requirements for your trusted employees, and stricter requirements like approval workflow for your partners when they request activation into their assigned group.

### Activate multiple role assignments in one request

With the privileged access groups preview, you can give workload-specific administrators quick access to multiple roles with a single just-in-time request. For example, your Tier 3 Office Admins might need just-in-time access to the Exchange Admin, Office Apps Admin, Teams Admin, and Search Admin roles to thoroughly investigate incidents daily. Before today it would require four consecutive requests, which are a process that takes some time. Instead, you can create a role assignable group called “Tier 3 Office Admins”, assign it to each of the four roles previously mentioned (or any Azure AD built-in roles) and enable it for Privileged Access in the group’s Activity section. Once enabled for privileged access, you can configure the just-in-time settings for members of the group and assign your admins and owners as eligible. When the admins elevate into the group, they’ll become members of all four Azure AD roles.

## Invite guest users and assign Azure resource roles in Privileged Identity Management

Azure Active Directory (Azure AD) guest users are part of the business-to-business (B2B) collaboration capabilities within Azure AD so that you can manage external guest users and vendors as guests in Azure AD. For example, you can use these Privileged Identity Management features for Azure identity tasks with guests such as assigning access to specific Azure resources, specifying assignment duration and end date, or requiring two-step verification on active assignment or activation. For more information on how to invite a guest to your organization and manage their access , see [Add B2B collaboration users in the Azure AD portal](../external-identities/add-users-administrator.md).

### When would you invite guests?

Here are a couple examples of when you might invite guests to your organization:

- Allow an external self-employed vendor that only has an email account to access your Azure resources for a project.
- Allow an external partner in a large organization that uses on-premises Active Directory Federation Services to access your expense application.
- Allow support engineers not in your organization (such as Microsoft support) to temporarily access your Azure resource to troubleshoot issues.

### How does collaboration using B2B guests work?

When you use B2B collaboration, you can invite an external user to your organization as a guest. The guest can be managed as a user in your organization, but a guest has to be authenticated in their home organization and not in your Azure AD organization. This means that if the guest no longer has access to their home organization, they also lose access to your organization. For example, if the guest leaves their organization, they automatically lose access to any resources you shared with them in Azure AD without you having to do anything. For more information about B2B collaboration, see [What is guest user access in Azure Active Directory B2B?](../external-identities/what-is-b2b.md).

![Diagram showing how a guest user is authenticated in their home directory](./media/pim-resource-roles-external-users/b2b-external-user.png)

## Next steps

- [License requirements to use Privileged Identity Management](subscription-requirements.md)
- [Securing privileged access for hybrid and cloud deployments in Azure AD](../roles/security-planning.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [Deploy Privileged Identity Management](pim-deployment-plan.md)

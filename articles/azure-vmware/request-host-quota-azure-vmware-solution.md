---
title: Request host quota for Azure VMware Solution
description: Learn how to request host quota/capacity for Azure VMware Solution. You can also request more hosts in an existing Azure VMware Solution private cloud.
ms.topic: how-to
ms.custom: contperf-fy21q3
ms.date: 05/13/2021
---

# Request host quota for Azure VMware Solution

In this how-to, you'll request host quota/capacity for [Azure VMware Solution](introduction.md). You'll submit a support ticket to have your hosts allocated. If you have an existing Azure VMware Solution private cloud and want more hosts allocated, you'll follow the same process.

>[!IMPORTANT]
>It can take a few days to allocate the hosts, depending on the number requested.  So request what is needed for provisioning, so you don't need to request a quota increase as often.

## Eligibility criteria

You'll need an Azure account in an Azure subscription. The Azure subscription must adhere to one of the following criteria:

- A subscription under an [Azure Enterprise Agreement (EA)](../cost-management-billing/manage/ea-portal-agreements.md) with Microsoft.
- A Cloud Solution Provider (CSP) managed subscription under an existing CSP Azure offers contract or an Azure plan.

## Request host quota for EA customers

1. In your Azure portal, under **Help + Support**, create a **[New support request](https://rc.portal.azure.com/#create/Microsoft.Support)** and provide the following information:
   - **Issue type:** Technical
   - **Subscription:** Select your subscription
   - **Service:** All services > Azure VMware Solution
   - **Resource:** General question 
   - **Summary:** Need capacity
   - **Problem type:** Capacity Management Issues
   - **Problem subtype:** Customer Request for Additional Host Quota/Capacity

1. In the **Description** of the support ticket, on the **Details** tab, provide information for:

   - POC or Production 
   - Region Name
   - Number of hosts
   - Any other details

   >[!NOTE]
   >Azure VMware Solution requires a minimum of three hosts and recommends redundancy of N+1 hosts. 

1. Select **Review + Create** to submit the request.


## Request host quota for CSP customers 

CSPs must use [Microsoft Partner Center](https://partner.microsoft.com) to enable Azure VMware Solution for their customers. This article uses [CSP Azure plan](/partner-center/azure-plan-lp) as an example to illustrate the purchase procedure for partners.

Access the Azure portal using the **Admin On Behalf Of** (AOBO) procedure from Partner Center.

>[!IMPORTANT] 
>Azure VMware Solution service does not provide a multi-tenancy required. Hosting partners requiring it are not supported. 

1. Configure the CSP Azure plan:

   1. In **Partner Center**, select **CSP** to access the **Customers** area.
   
      :::image type="content" source="media/enable-azure-vmware-solution/csp-customers-screen.png" alt-text="Microsoft Partner Center customers area" lightbox="media/enable-azure-vmware-solution/csp-customers-screen.png":::
   
   1. Select your customer and then select **Add products**.
   
      :::image type="content" source="media/enable-azure-vmware-solution/csp-partner-center.png" alt-text="Microsoft Partner Center" lightbox="media/enable-azure-vmware-solution/csp-partner-center.png":::
   
   1. Select **Azure plan** and then select **Add to cart**. 
   
   1. Review and finish the general setup of the Azure plan subscription for your customer. For more information, see [Microsoft Partner Center documentation](/partner-center/azure-plan-manage).

1. After you configure the Azure plan and you have the needed [Azure RBAC permissions](/partner-center/azure-plan-manage) in place for the subscription, you'll request the quota for your Azure plan subscription. 

   1. Access Azure portal from [Microsoft Partner Center](https://partner.microsoft.com) using the **Admin On Behalf Of** (AOBO) procedure.
   
   1. Select **CSP** to access the **Customers** area.
   
   1. Expand customer details and select **Microsoft Azure Management Portal**.
   
   1. In Azure portal, under **Help + Support**, create a **[New support request](https://rc.portal.azure.com/#create/Microsoft.Support)** and provide the following information:
      - **Issue type:** Technical
      - **Subscription:** Select your subscription
      - **Service:** All services > Azure VMware Solution
      - **Resource:** General question 
      - **Summary:** Need capacity
      - **Problem type:** Capacity Management Issues
      - **Problem subtype:** Customer Request for Additional Host Quota/Capacity
   
   1. In the **Description** of the support ticket, on the **Details** tab, provide information for:
   
      - POC or Production 
      - Region Name
      - Number of hosts
      - Any other details
      - Is intended to host multiple customers?
   
      >[!NOTE]
      >Azure VMware Solution requires a minimum of three hosts and recommends redundancy of N+1 hosts. 
   
   1. Select **Review + Create** to submit the request.


## Next steps

Before you can deploy Azure VMware Solution, you must first [register the resource provider](deploy-azure-vmware-solution.md#step-1-register-the-microsoftavs-resource-provider) with your subscription to enable the service.   


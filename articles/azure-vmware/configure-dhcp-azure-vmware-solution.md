---
title: Configure DHCP for Azure VMware Solution
description: Learn how to configure DHCP by using either NSX-T Manager to host a DHCP server or use a third-party external DHCP server.
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.date: 05/28/2021

# Customer intent: As an Azure service administrator, I want to configure DHCP by using either NSX-T Manager to host a DHCP server or use a third-party external DHCP server.

---

# Configure DHCP for Azure VMware Solution

[!INCLUDE [dhcp-dns-in-azure-vmware-solution-description](includes/dhcp-dns-in-azure-vmware-solution-description.md)]

In this how-to article, you'll use NSX-T Manager to configure DHCP for Azure VMware Solution in one of the following two ways: 

- [NSX-T to host your DHCP server](#use-nsx-t-to-host-your-dhcp-server)

- [Third-party external DHCP server](#use-a-third-party-external-dhcp-server)

>[!TIP]
>If you want to configure DHCP using a simplified view of NSX-T operations, see [Create a DHCP server or DHCP relay using the Azure portal](configure-nsx-network-components-azure-portal.md#create-a-dhcp-server-or-dhcp-relay-using-the-azure-portal). The simplified view is targeted at users unfamiliar with NSX-T Manager. 



>[!IMPORTANT]
>DHCP does not work for virtual machines (VMs) on the VMware HCX L2 stretch network when the DHCP server is in the on-premises datacenter.  NSX, by default, blocks all DHCP requests from traversing the L2 stretch. For the solution, see the [Configure DHCP on L2 stretched VMware HCX networks](configure-l2-stretched-vmware-hcx-networks.md) procedure.


## Use NSX-T to host your DHCP server
If you want to use NSX-T to host your DHCP server, you'll create a DHCP server and a relay service. Then you'll add a network segment and specify the DHCP IP address range.

### Create a DHCP server

1. In NSX-T Manager, select **Networking** > **DHCP**, and then select **Add Server**.

1. Select **DHCP** for the **Server Type**, provide the server name and IP address, and then select **Save**.

   :::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="Screenshot showing how to add a DHCP server in NSX-T Manager." border="true":::

1. Select **Tier 1 Gateways**, select the vertical ellipsis on the Tier-1 gateway, and then select **Edit**.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="Screenshot showing how to edit the Tier-1 Gateway for using a DHCP server." border="true":::

1. Select **No IP Allocation Set** to add a subnet.

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="Screenshot showing how to add a subnet to the Tier-1 Gateway for using a DHCP server." border="true":::

1. For **Type**, select **DHCP Local Server**. 
   
1. For the **DHCP Server**, select **Default DHCP**, and then select **Save**.

1. Select **Save** again and then select **Close Editing**.

### Add a network segment

[!INCLUDE [add-network-segment-steps](includes/add-network-segment-steps.md)]

### Specify the DHCP IP address range
 
When you create a relay to a DHCP server, you'll also specify the DHCP IP address range.

>[!NOTE]
>The IP address range shouldn't overlap with the IP range used in other virtual networks in your subscription and on-premises networks.

1. In NSX-T Manager, select **Networking** > **Segments**. 
   
1. Select the vertical ellipsis on the segment name and select **Edit**.
   
1. Select **Set Subnets** to specify the DHCP IP address for the subnet. 
   
   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="Screenshot showing how to set the subnets to specify the DHCP IP address  for using a DHCP server." border="true":::
      
1. Modify the gateway IP address if needed, and enter the DHCP range IP. 
      
   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="Screenshot showing the gateway IP address and DHCP ranges for using a DHCP server." border="true":::
      
1. Select **Apply**, and then **Save**. The segment is assigned a DHCP server pool.
      
   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="Screenshot showing that the DHCP server pool assigned to segment for using a DHCP server." border="true":::


## Use a third-party external DHCP server

If you want to use a third-party external DHCP server, you'll create a DHCP relay service in NSX-T Manager. You'll also specify the DHCP IP address range.


### Create DHCP relay service

Use a DHCP relay for any non-NSX based DHCP service. For example, a VM running DHCP in Azure VMware Solution, Azure IaaS, or on-premises.

1. In NSX-T Manager, select **Networking** > **DHCP**, and then select **Add Server**.

1. Select **DHCP Relay** for the **Server Type**, provide the server name and IP address, and then select **Save**.

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="Screenshot showing how to create a DHCP relay service in NSX-T Manager." border="true":::

1. Select **Tier 1 Gateways**, select the vertical ellipsis on the Tier-1 gateway, and then select **Edit**.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="Screenshot showing how to edit the Tier-1 Gateway." border="true":::

1. Select **No IP Allocation Set** to define the IP address allocation.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="Screenshot showing how to add a subnet to the Tier-1 Gateway." border="true":::

1. For **Type**, select **DHCP Server**. 
   
1. For the **DHCP Server**, select **DHCP Relay**, and then select **Save**.

1. Select **Save** again and then select **Close Editing**.


### Specify the DHCP IP address range

When you create a relay to a DHCP server, you'll also specify the DHCP IP address range.

>[!NOTE]
>The IP address range shouldn't overlap with the IP range used in other virtual networks in your subscription and on-premises networks.

1. In NSX-T Manager, select **Networking** > **Segments**. 
   
1. Select the vertical ellipsis on the segment name and select **Edit**.
   
1. Select **Set Subnets** to specify the DHCP IP address for the subnet. 
   
   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="Screenshot showing how to set the subnets to specify the DHCP IP address." border="true":::
      
1. Modify the gateway IP address if needed, and enter the DHCP range IP. 
      
   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="Screenshot showing the gateway IP address and DHCP ranges." border="true":::
      
1. Select **Apply**, and then **Save**. The segment is assigned a DHCP server pool.
      
   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="Screenshot showing that the DHCP server pool assigned to segment." border="true":::


## Next steps

If you want to send DHCP requests from your Azure VMware Solution VMs to a non-NSX-T DHCP server, see the [Configure DHCP on L2 stretched VMware HCX networks](configure-l2-stretched-vmware-hcx-networks.md) procedure.

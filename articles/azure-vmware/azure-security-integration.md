---
title: Integrate Azure Security Center with Azure VMware Solution 
description: Learn how to protect your Azure VMware Solution VMs with Azure's native security tools from the Azure Security Center dashboard.
ms.topic: how-to
ms.date: 06/14/2021
---

# Integrate Azure Security Center with Azure VMware Solution 

Azure Security Center provides advanced threat protection across your Azure VMware Solution and on-premises virtual machines (VMs). It assesses the vulnerability of Azure VMware Solution VMs and raise alerts as needed. These security alerts can be forwarded to Azure Monitor for resolution. You can define security policies in Azure Security Center. For more information, see [Working with security policies](../security-center/tutorial-security-policy.md). 

Azure Security Center offers many features, including:
- File integrity monitoring
- Fileless attack detection
- Operating system patch assessment 
- Security misconfigurations assessment
- Endpoint protection assessment

The diagram shows the integrated monitoring architecture of integrated security for Azure VMware Solution VMs.
 
:::image type="content" source="media/azure-security-integration/azure-integrated-security-architecture.png" alt-text="Diagram showing the architecture of Azure Integrated Security." border="false":::


## Prerequisites

- [Plan for optimized use of Security Center](../security-center/security-center-planning-and-operations-guide.md).

- [Review the supported platforms in Security Center](../security-center/security-center-os-coverage.md).

- [Create a Log Analytics workspace](../azure-monitor/logs/quick-create-workspace.md) to collect data from various sources.

- [Enable Azure Security Center in your subscription](../security-center/security-center-get-started.md). 

   >[!NOTE]
   >Azure Security Center is a pre-configured tool that doesn't require deployment, but you'll need to enable it.

- [Enable Azure Defender](../security-center/enable-azure-defender.md). 


## Add Azure VMware Solution VMs to Security Center

1. In the Azure portal, search on **Azure Arc** and select it.

2. Under Resources, select **Servers** and then **+Add**.

   :::image type="content" source="media/azure-security-integration/add-server-to-azure-arc.png" alt-text="A screenshot showing Azure Arc Servers page for adding an Azure VMware Solution VM to Azure.":::

3. Select **Generate script**.
 
   :::image type="content" source="media/azure-security-integration/add-server-using-script.png" alt-text="A screenshot of Azure Arc page showing option for adding a server using interactive script."::: 
 
4. On the **Prerequisites** tab, select **Next**.

5. On the **Resource details** tab, fill in the following details and then select **Next: Tags**. 

    - Subscription

    - Resource group

    - Region 

    - Operating system

    - Proxy Server details
    
6. On the **Tags** tab, select **Next**.

7. On the **Download and run script** tab, select **Download**.

8. Specify your operating system and run the script on your Azure VMware Solution VM.

## View recommendations and passed assessments

This provides you with the security health details of your resource. 

1. In Azure Security Center, select **Inventory** from the left pane.

2. For Resource type, select **Servers - Azure Arc**.
 
   :::image type="content" source="media/azure-security-integration/select-resource-in-security-center.png" alt-text="A screenshot of the Azure Security Center Inventory page showing Servers - Azure Arc selected under Resource type.":::

3. Select the name of your resource. A page opens showing the security health details of your resource.

4. Under **Recommendation list**, select the **Recommendations**, **Passed assessments**, and **Unavailable assessments** tabs to view these details.

   :::image type="content" source="media/azure-security-integration/view-recommendations-assessments.png" alt-text="A screenshot of Azure Security Center showing security recommendations and assessments.":::

## Deploy an Azure Sentinel workspace

Azure Sentinel is built on top of a Log Analytics workspace, so you'll just need to select the Log Analytics workspace you want to use.

1. In the Azure portal, search for **Azure Sentinel**, and select it.

2. On the Azure Sentinel workspaces page, select **+Add**.

3. Select the Log Analytics workspace and select **Add**.

## Enable data collector for security events

1. On the Azure Sentinel workspaces page, select the configured workspace.

2. Under Configuration, select **Data connectors**.

3. Under the Connector Name column, select **Security Events** from the list, and then select **Open connector page**.

4. On the connector page, select the events you wish to stream and then select **Apply Changes**.

   :::image type="content" source="media/azure-security-integration/select-events-you-want-to-stream.png" alt-text="Screenshot of Security Events page in Azure Sentinel where you can select which events to stream.":::

## Connect Azure Sentinel with Azure Security Center  

1. On the Azure Sentinel workspace page, select the configured workspace.

2. Under Configuration, select **Data connectors**.

3. Select **Azure Security Center** from the list and then select **Open connector page**.

    :::image type="content" source="media/azure-security-integration/connect-security-center-with-azure-sentinel.png" alt-text="Screenshot of Data connectors page in Azure Sentinel showing selection to connect Azure Security Center with Azure Sentinel.":::

4. Select **Connect** to connect the Azure Security Center with Azure Sentinel.

5. Enable **Create incident** to generate an incident for Azure Security Center.

## Create rules to identify security threats

After connecting data sources to Azure Sentinel, you can create rules to generate alerts for detected threats. In the following example, we'll create a rule for attempts to sign in to Windows server with the wrong password.

1. On the Azure Sentinel overview page, under Configurations, select **Analytics**.

2. Under Configurations, select **Analytics**.

3. Select **+Create** and on the drop-down, select **Scheduled query rule**.

4. On the **General** tab, enter the required information and then select **Next: Set rule logic**.

    - Name

    - Description

    - Tactics

    - Severity

    - Status

5. On the **Set rule logic** tab, enter the required information and then select **Next**.

    - Rule query (here showing our example query)
    
        ```
        SecurityEvent
        |where Activity startswith '4625'
        |summarize count () by IpAddress,Computer
        |where count_ > 3
        ```
        
    - Map entities

    - Query scheduling

    - Alert threshold

    - Event grouping

    - Suppression


6. On the **Incident settings** tab, enable **Create incidents from alerts triggered by this analytics rule** and select **Next: Automated response**.
 
    :::image type="content" source="media/azure-security-integration/create-new-analytic-rule-wizard.png" alt-text="Screenshot of the Analytic rule wizard for creating a new rule in Azure Sentinel. Shows Create incidents from alerts triggered by this rule as enabled.":::

7. Select **Next: Review**.

8. On the **Review and create** tab, review the information and select **Create**.

>[!TIP]
>After the third failed attempt to sign in to Windows server, the created rule triggers an incident for every unsuccessful attempt.

## View alerts

You can view generated incidents with Azure Sentinel. You can also assign incidents and close them once they're resolved, all from within Azure Sentinel.

1. Go to the Azure Sentinel overview page.

2. Under Threat Management, select **Incidents**.

3. Select an incident and then assign it to a team for resolution.

    :::image type="content" source="media/azure-security-integration/assign-incident.png" alt-text="Screenshot of Azure Sentinel Incidents page with incident selected and option to assign the incident for resolution.":::

>[!TIP]
>After resolving the issue, you can close it.

## Hunt security threats with queries

You can create queries or use the available pre-defined query in Azure Sentinel to identify threats in your environment. The following steps run a pre-defined query.

1. On the Azure Sentinel overview page, under Threat management, select **Hunting**. A list of pre-defined queries is displayed.

   >[!TIP]
   >You can also create a new query by selecting **+New Query**. 
   >
   >:::image type="content" source="media/azure-security-integration/create-new-query.png" alt-text="Screenshot of Azure Sentinel Hunting page with + New Query highlighted.":::

3. Select a query and then select **Run Query**.

4. Select **View Results** to check the results.



## Next steps

Now that you've covered how to protect your Azure VMware Solution VMs, you may want to learn about:

- [Using the Azure Defender dashboard](../security-center/azure-defender-dashboard.md)
- [Advanced multistage attack detection in Azure Sentinel](../azure-monitor/logs/quick-create-workspace.md)
- [Integrating Azure native services in Azure VMware Solution](integrate-azure-native-services.md)

---
title: Azure Automation Update Management Supported Clients
description: This article describes the supported Windows and Linux operating systems with Azure Automation Update Management.
services: automation
ms.subservice: update-management
ms.date: 06/07/2021
ms.topic: conceptual
---

# Operating systems supported by Update Management

This article details the Windows and Linux operating systems supported and system requirements for machines or servers managed by Update Management.

## Supported operating systems

The following table lists the supported operating systems for update assessments and patching. Patching requires a system Hybrid Runbook Worker, which is automatically installed when you enable the virtual machine or server for management by Update Management. For information on Hybrid Runbook Worker system requirements, see [Deploy a Windows Hybrid Runbook Worker](../automation-windows-hrw-install.md#prerequisites) and [Deploy a Linux Hybrid Runbook Worker](../automation-linux-hrw-install.md#prerequisites).

> [!NOTE]
> Update assessment of Linux machines is only supported in certain regions as listed in the Automation account and Log Analytics workspace [mappings table](../how-to/region-mappings.md#supported-mappings).

|Operating system  |Notes  |
|---------|---------|
|Windows Server 2019 (Datacenter/Standard including Server Core)<br><br>Windows Server 2016 (Datacenter/Standard excluding Server Core)<br><br>Windows Server 2012 R2(Datacenter/Standard)<br><br>Windows Server 2012 | |
|Windows Server 2008 R2 (RTM and SP1 Standard)| Update Management supports assessments and patching for this operating system. The [Hybrid Runbook Worker](../automation-windows-hrw-install.md) is supported for Windows Server 2008 R2. |
|CentOS 6, 7, and 8 (x64)      | Linux agents require access to an update repository. Classification-based patching requires `yum` to return security data that CentOS doesn't have in its RTM releases. For more information on classification-based patching on CentOS, see [Update classifications on Linux](view-update-assessments.md#linux).          |
|Red Hat Enterprise 6, 7, and 8 (x64)     | Linux agents require access to an update repository.        |
|SUSE Linux Enterprise Server 12, 15, and 15.1 (x64)     | Linux agents require access to an update repository.     |
|Ubuntu 14.04 LTS, 16.04 LTS, and 18.04 LTS (x64)      |Linux agents require access to an update repository.         |

> [!NOTE]
> Update Management does not support safely automating update management across all instances in an Azure virtual machine scale set. [Automatic OS image upgrades](../../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md) is the recommended method for managing OS image upgrades on your scale set.

## Unsupported operating systems

The following table lists operating systems not supported by Update Management:

|Operating system  |Notes  |
|---------|---------|
|Windows client     | Client operating systems (such as Windows 7 and Windows 10) aren't supported.<br> For Azure Windows Virtual Desktop (WVD), the recommended method<br> to manage updates is [Microsoft Endpoint Configuration Manager](../../virtual-desktop/configure-automatic-updates.md) for Windows 10 client machine patch management. |
|Windows Server 2016 Nano Server     | Not supported.       |
|Azure Kubernetes Service Nodes | Not supported. Use the patching process described in [Apply security and kernel updates to Linux nodes in Azure Kubernetes Service (AKS)](../../aks/node-updates-kured.md)|

## System requirements

The following information describes operating system-specific requirements. For additional guidance, see [Network planning](plan-deployment.md#ports). To understand requirements for TLS 1.2, see [TLS 1.2 enforcement for Azure Automation](../automation-managing-data.md#tls-12-enforcement-for-azure-automation).

### Windows

Software Requirements:

- .NET Framework 4.6 or later is required. ([Download the .NET Framework](/dotnet/framework/install/guide-for-developers).
- Windows PowerShell 5.1 is required ([Download Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616).)
- The Update Management feature depends on the system Hybrid Runbook Worker role, and you should confirm its [system requirements](../automation-windows-hrw-install.md#prerequisites).

Windows Update agents must be configured to communicate with a Windows Server Update Services (WSUS) server, or they require access to Microsoft Update. For hybrid machines, we recommend installing the Log Analytics agent for Windows by first connecting your machine to [Azure Arc enabled servers](../../azure-arc/servers/overview.md), and then use Azure Policy to assign the [Deploy Log Analytics agent to Windows Azure Arc machines](../../governance/policy/samples/built-in-policies.md#monitoring) built-in policy. Alternatively, if you plan to monitor the machines with VM insights, instead use the [Enable Enable VM insights](../../governance/policy/samples/built-in-initiatives.md#monitoring) initiative.

You can use Update Management with Microsoft Endpoint Configuration Manager. To learn more about integration scenarios, see [Integrate Update Management with Windows Endpoint Configuration Manager](mecmintegration.md). The [Log Analytics agent for Windows](../../azure-monitor/agents/agent-windows.md) is required for Windows servers managed by sites in your Configuration Manager environment.

By default, Windows VMs that are deployed from Azure Marketplace are set to receive automatic updates from Windows Update Service. This behavior doesn't change when you add Windows VMs to your workspace. If you don't actively manage updates by using Update Management, the default behavior (to automatically apply updates) applies.

> [!NOTE]
> You can modify Group Policy so that machine reboots can be performed only by the user, not by the system. Managed machines can get stuck if Update Management doesn't have rights to reboot the machine without manual interaction from the user. For more information, see [Configure Group Policy settings for Automatic Updates](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

### Linux

Software Requirements:

- The machine requires access to an update repository, either private or public.
- TLS 1.1 or TLS 1.2 is required to interact with Update Management.
- The Update Management feature depends on the system Hybrid Runbook Worker role, and you should confirm its [system requirements](../automation-linux-hrw-install.md#prerequisites). Because Update Management uses Automation runbooks to initiate assessment and update of your machines, review the [version of Python required](../automation-linux-hrw-install.md#supported-runbook-types) for your supported Linux distro.

> [!NOTE]
> Update assessment of Linux machines is only supported in certain regions. See the Automation account and Log Analytics workspace [mappings table](../how-to/region-mappings.md#supported-mappings).

For hybrid machines, we recommend installing the Log Analytics agent for Linux by first connecting your machine to [Azure Arc enabled servers](../../azure-arc/servers/overview.md), and then use Azure Policy to assign the [Deploy Log Analytics agent to Linux Azure Arc machines](../../governance/policy/samples/built-in-policies.md#monitoring) built-in policy. Alternatively, if you plan to monitor the machines with Azure Monitor for VMs, instead use the [Enable Azure Monitor for VMs](../../governance/policy/samples/built-in-initiatives.md#monitoring) initiative.

## Next steps

Before you enable and use Update Management, review [Plan your Update Management deployment](plan-deployment.md).
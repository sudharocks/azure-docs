---
title: Using Microsoft Defender for Endpoint in Microsoft Defender for Cloud to protect native, on-premises, and AWS machines.
description: Learn about deploying Microsoft Defender for Endpoint from Microsoft Defender for Cloud to protect Azure, hybrid, and multicloud machines.
author: bmansheim
ms.author: benmansheim
ms.topic: how-to
ms.date: 01/15/2023
---

# Protect your endpoints with Defender for Cloud's integrated EDR solution: Microsoft Defender for Endpoint

With Microsoft Defender for Servers, you gain access to and can deploy [Microsoft Defender for Endpoint](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint) to your server resources. Microsoft Defender for Endpoint is a holistic, cloud-delivered, endpoint security solution. The main features include:

- Risk-based vulnerability management and assessment
- Attack surface reduction
- Behavioral based and cloud-powered protection
- Endpoint detection and response (EDR)
- Automatic investigation and remediation
- Managed hunting services

You can learn about Defender for Cloud's integration with Microsoft Defender for Endpoint by watching this video from the Defender for Cloud in the Field video series: [Defender for Servers integration with Microsoft Defender for Endpoint](episode-sixteen.md)

For more information about migrating servers from Defender for Endpoint to Defender for Cloud, see the [Microsoft Defender for Endpoint to Microsoft Defender for Cloud Migration Guide](/microsoft-365/security/defender-endpoint/migrating-mde-server-to-cloud).

## Availability

| Aspect | Details |
|---|:--|
| Release state: | General availability (GA) |
| Pricing: | Requires [Microsoft Defender for Servers Plan 1 or Plan 2](plan-defender-for-servers-select-plan.md#plan-features) |
| Supported environments: | :::image type="icon" source="./media/icons/yes-icon.png"::: Azure Arc-enabled machines running Windows/Linux<br>:::image type="icon" source="./media/icons/yes-icon.png":::Azure VMs running Linux ([supported versions](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux))<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Azure VMs running Windows Server 2022, 2019, 2016, 2012 R2, 2008 R2 SP1, [Windows 10/11 Enterprise multi-session](../virtual-desktop/windows-10-multisession-faq.yml) (formerly Enterprise for Virtual Desktops)<br>:::image type="icon" source="./media/icons/no-icon.png"::: Azure VMs running Windows 10 or Windows 11 (except if running Windows 10/11 Enterprise multi-session) |
| Required roles and permissions: | - To enable/disable the integration: **Security admin** or **Owner**<br>- To view Defender for Endpoint alerts in Defender for Cloud: **Security reader**, **Reader**, **Resource Group Contributor**, **Resource Group Owner**, **Security admin**, **Subscription owner**, or **Subscription Contributor** |
| Clouds: | :::image type="icon" source="./media/icons/yes-icon.png"::: Commercial clouds<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Azure Government<br>:::image type="icon" source="./media/icons/no-icon.png"::: Azure China 21Vianet <br>:::image type="icon" source="./media/icons/yes-icon.png"::: Connected AWS accounts     <br>:::image type="icon" source="./media/icons/yes-icon.png"::: Connected GCP projects |

## Benefits of integrating Microsoft Defender for Endpoint with Defender for Cloud

[Microsoft Defender for Endpoint](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint) protects your Windows and Linux machines whether they're hosted in Azure, hybrid clouds (on-premises), or multicloud environments.

The protections include:

- **Advanced post-breach detection sensors**. Defenders for Endpoint's sensors collect a vast array of behavioral signals from your machines.

- **Vulnerability assessment from the Microsoft threat and vulnerability management solution**. With Microsoft Defender for Endpoint installed, Defender for Cloud can show vulnerabilities discovered by the threat and vulnerability management module and also offer this module as a supported vulnerability assessment solution. Learn more in [Investigate weaknesses with Microsoft Defender for Endpoint's threat and vulnerability management](deploy-vulnerability-assessment-defender-vulnerability-management.md).

    This module also brings the software inventory features described in [Access a software inventory](asset-inventory.md#access-a-software-inventory) and can be automatically enabled for supported machines with [the auto deploy settings](auto-deploy-vulnerability-assessment.md).

- **Analytics-based, cloud-powered, post-breach detection**. Defender for Endpoint quickly adapts to changing threats. It uses advanced analytics and big data. It's amplified by the power of the Intelligent Security Graph with signals across Windows, Azure, and Office to detect unknown threats. It provides actionable alerts and enables you to respond quickly.

- **Threat intelligence**. Defender for Endpoint generates alerts when it identifies attacker tools, techniques, and procedures. It uses data generated by Microsoft threat hunters and security teams, augmented by intelligence provided by partners.

When you integrate Defender for Endpoint with Defender for Cloud, you'll gain access to the benefits from the following extra capabilities:

- **Automated onboarding**. Defender for Cloud automatically enables the Defender for Endpoint sensor on all supported machines connected to Defender for Cloud.

- **Single pane of glass**. The Defender for Cloud portal pages displays Defender for Endpoint alerts. To investigate further, use Microsoft Defender for Endpoint's own portal pages where you'll see additional information such as the alert process tree and the incident graph. You can also see a detailed machine timeline that shows every behavior for a historical period of up to six months.

    :::image type="content" source="./media/integration-defender-for-endpoint/microsoft-defender-security-center.png" alt-text="Microsoft Defender for Endpoint's own Security Center" lightbox="./media/integration-defender-for-endpoint/microsoft-defender-security-center.png":::

## What are the requirements for the Microsoft Defender for Endpoint tenant?

A Defender for Endpoint tenant is automatically created, when you use Defender for Cloud to monitor your machines.

- **Location:** Data collected by Defender for Endpoint is stored in the geo-location of the tenant as identified during provisioning. Customer data - in pseudonymized form - may also be stored in the central storage and processing systems in the United States. After you've configured the location, you can't change it. If you have your own license for Microsoft Defender for Endpoint and need to move your data to another location, [contact Microsoft support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) to reset the tenant.

- **Moving subscriptions:** If you've moved your Azure subscription between Azure tenants, some manual preparatory steps are required before Defender for Cloud will deploy Defender for Endpoint. For full details, [contact Microsoft support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## Enable the Microsoft Defender for Endpoint integration

### Prerequisites

Before you can enable the Microsoft Defender for Endpoint integration with Defender for Cloud, you must confirm that your machine meets the necessary requirements for Defender for Endpoint:

- Ensure the machine is connected to Azure and the internet as required:

    - **Azure virtual machines (Windows or Linux)** - Configure the network settings described in configure device proxy and internet connectivity settings: [Windows](/microsoft-365/security/defender-endpoint/configure-proxy-internet) or [Linux](/microsoft-365/security/defender-endpoint/linux-static-proxy-configuration).

    - **On-premises machines** - Connect your target machines to Azure Arc as explained in [Connect hybrid machines with Azure Arc-enabled servers](../azure-arc/servers/learn/quick-enable-hybrid-vm.md).

- Enable **Microsoft Defender for Servers**. See [Quickstart: Enable Defender for Cloud's enhanced security features](enable-enhanced-security.md).

    > [!IMPORTANT]
    > Defender for Cloud's integration with Microsoft Defender for Endpoint is enabled by default. So when you enable enhanced security features, you give consent for Microsoft Defender for Servers to access the Microsoft Defender for Endpoint data related to vulnerabilities, installed software, and alerts for your endpoints.

- For Windows servers, make sure that your servers meet the requirements for [onboarding Microsoft Defender for Endpoint](/microsoft-365/security/defender-endpoint/configure-server-endpoints#windows-server-2012-r2-and-windows-server-2016).

- For Linux servers, you must have Python installed. Python 3 is recommended for all distros, but is required for RHEL 8.x and Ubuntu 20.04 or higher. If needed, see Step-by-step Instructions for Installing Python on Linux.

- If you've moved your subscription between Azure tenants, some manual preparatory steps are also required. For details, [contact Microsoft support](https://portal.azure.com/#view/Microsoft_Azure_Support/HelpAndSupportBlade/~/overview).

### Enable the integration

- On [Windows](#windows)

- On [Linux](#linux)

#### Windows

[The MDE unified solution](/microsoft-365/security/defender-endpoint/configure-server-endpoints#new-windows-server-2012-r2-and-2016-functionality-in-the-modern-unified-solution) doesn't use or require installation of the Log Analytics agent. The unified solution is automatically deployed for Azure Windows 2012 R2 and 2016 servers, Windows servers connected through Azure Arc, and Windows multicloud servers connected through the multicloud connectors.

You'll deploy Defender for Endpoint to your Windows machines in one of two ways - depending on whether you've already deployed it to your Windows machines:

- [Users with Defender for Servers enabled and Microsoft Defender for Endpoint deployed](#users-with-defender-for-servers-enabled-and-microsoft-defender-for-endpoint-deployed)
- [Users who never enabled the integration with Microsoft Defender for Endpoint](#users-who-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows)

##### Users with Defender for Servers enabled and Microsoft Defender for Endpoint deployed

If you've already enabled the integration with **Defender for Endpoint**, you have complete control over when and whether to deploy the MDE unified solution to your **Windows** machines.

To deploy the MDE unified solution, you'll need to use the [REST API call](#enable-the-mde-unified-solution-at-scale) or the Azure portal:

1. From Defender for Cloud's menu, select **Environment settings** and select the subscription with the Windows machines that you want to receive Defender for Endpoint.

1. In the Monitoring coverage column of the Defender for Servers plan, select **Settings**.

    The status of the Endpoint protections component is **Partial**, meaning that not all parts of the component are enabled.

    > [!NOTE]
    > If the status is **Off**, use the instructions in [Users who've never enabled the integration with Microsoft Defender for Endpoint for Windows](#users-who-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows).

1. Select **Fix** to see the components that aren't enabled.


    :::image type="content" source="./media/integration-defender-for-endpoint/fix-defender-for-endpoint.png" alt-text="Screenshot of Fix button that enables Microsoft Defender for Endpoint support.":::

1. To enable the Unified solution for Windows Server 2012 R2 and 2016 machines, select **Enable**.

    :::image type="content" source="./media/integration-defender-for-endpoint/enable-defender-for-endpoint-unified.png" alt-text="Screenshot of enabling the use of the MDE unified solution for Windows Server 2012 R2 and 2016 machines.":::

1. To save the changes, select **Save** at the top of the page and then select **Continue** in the Settings and monitoring page.

Microsoft Defender for Cloud will:

- Stop the existing MDE process in the Log Analytics agent that collects data for Defender for Servers.
- Install the MDE unified solution for all existing and new Windows Server 2012 R2 and 2016 machines.

Microsoft Defender for Cloud will automatically onboard your machines to Microsoft Defender for Endpoint. Onboarding might take up to 12 hours. For new machines created after the integration has been enabled, onboarding takes up to an hour.

> [!NOTE]
> If you choose not to deploy the MDE unified solution to your Windows 2012 R2 and 2016 servers in Defender for Servers Plan 2 and then downgrade Defender for Servers to Plan 1, the MDE unified solution is not deployed to those servers so that your existing deployment is not changed without your explicit consent.

##### Users who never enabled the integration with Microsoft Defender for Endpoint for Windows

If you've never enabled the integration for Windows, Endpoint protection enables Defender for Cloud to deploy Defender for Endpoint to *both* your Windows and Linux machines.

To deploy the MDE unified solution, you'll need to use the [REST API call](#enable-the-mde-unified-solution-at-scale) or the Azure portal:

1. From Defender for Cloud's menu, select **Environment settings** and select the subscription with the machines that you want to receive Defender for Endpoint.

1. In the status of the Endpoint protection component, select **On** to enable the integration with Microsoft Defender for Endpoint.

    :::image type="content" source="media/integration-defender-for-endpoint/enable-defender-for-endpoint.png" alt-text="Screenshot of Status toggle that enables Microsoft Defender for Endpoint." lightbox="media/integration-defender-for-endpoint/enable-defender-for-endpoint.png":::

The MDE agent unified solution is deployed to all of the machines in the selected subscription.

#### Linux

You'll deploy Defender for Endpoint to your Linux machines in one of these ways, depending on whether you've already deployed it to your Windows machines:

- Enable for a specific subscription in the Azure portal environment settings
  - [Existing users with Defender for Cloud's enhanced security features enabled and Microsoft Defender for Endpoint for Windows](#existing-users-with-defender-for-clouds-enhanced-security-features-enabled-and-microsoft-defender-for-endpoint-for-windows)
  - [New users who never enabled the integration with Microsoft Defender for Endpoint for Windows](#new-users-who-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows)
- [Enable for multiple subscriptions in the Azure portal dashboard](#enable-for-multiple-subscriptions-in-the-azure-portal-dashboard)
- Enable for multiple subscriptions with a PowerShell script


##### Existing users with Defender for Cloud's enhanced security features enabled and Microsoft Defender for Endpoint for Windows

If you've already enabled the integration with **Defender for Endpoint for Windows**, you have complete control over when and whether to deploy Defender for Endpoint to your **Linux** machines.

1. From Defender for Cloud's menu, select **Environment settings** and select the subscription with the Linux machines that you want to receive Defender for Endpoint.

1. In the Monitoring coverage column of the Defender for Server plan, select **Settings**.

    The status of the Endpoint protections component is **Partial**, meaning that not all parts of the component are enabled.

    > [!NOTE]
    > If the status is **Off** isn't selected, use the instructions in [Users who've never enabled the integration with Microsoft Defender for Endpoint for Windows](#users-who-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows).

1. Select **Fix** to see the components that aren't enabled.


    :::image type="content" source="./media/integration-defender-for-endpoint/fix-defender-for-endpoint.png" alt-text="Screenshot of Fix button that enables Microsoft Defender for Endpoint support.":::

1. To enable deployment to Linux machines, select **Enable**.

    :::image type="content" source="./media/integration-defender-for-endpoint/enable-defender-for-endpoint-linux.png" alt-text="Screenshot of enabling the integration between Defender for Cloud and Microsoft's EDR solution, Microsoft Defender for Endpoint for Linux.":::

1. To save the changes, select **Save** at the top of the page and then select **Continue** in the Settings and monitoring page.

    Microsoft Defender for Cloud will:

    - Automatically onboard your Linux machines to Defender for Endpoint
    - Ignore any machines that are running other fanotify-based solutions (see details of the `fanotify` kernel option required in [Linux system requirements](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux#system-requirements))
    - Detect any previous installations of Defender for Endpoint and reconfigure them to integrate with Defender for Cloud

    Microsoft Defender for Cloud will automatically onboard your machines to Microsoft Defender for Endpoint. Onboarding might take up to 12 hours. For new machines created after the integration has been enabled, onboarding takes up to an hour.

    > [!NOTE]
    > The next time you return to this page of the Azure portal, the **Enable for Linux machines** button won't be shown. To disable the integration for Linux, you'll need to disable it for Windows too by clearing the checkbox for **Allow Microsoft Defender for Endpoint to access my data**, and selecting **Save**.


1. To verify installation of Defender for Endpoint on a Linux machine, run the following shell command on your machines:

    `mdatp health`

    If Microsoft Defender for Endpoint is installed, you'll see its health status:

    `healthy : true`

    `licensed: true`

    Also, in the Azure portal you'll see a new Azure extension on your machines called `MDE.Linux`.

##### New users who never enabled the integration with Microsoft Defender for Endpoint for Windows

If you've never enabled the integration for Windows, endpoint protection enables Defender for Cloud to deploy Defender for Endpoint to *both* your Windows and Linux machines.

1. From Defender for Cloud's menu, select **Environment settings** and select the subscription with the Linux machines that you want to receive Defender for Endpoint.

1. In the status of the Endpoint protection component, select **On** to enable the integration with Microsoft Defender for Endpoint.

    :::image type="content" source="media/integration-defender-for-endpoint/enable-defender-for-endpoint.png" alt-text="Screenshot of Status toggle that enables Microsoft Defender for Endpoint." lightbox="media/integration-defender-for-endpoint/enable-defender-for-endpoint.png":::

    Microsoft Defender for Cloud will:

    - Automatically onboard your Windows and Linux machines to Defender for Endpoint
    - Ignore any Linux machines that are running other fanotify-based solutions (see details of the `fanotify` kernel option required in [Linux system requirements](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux#system-requirements))
    - Detect any previous installations of Defender for Endpoint and reconfigure them to integrate with Defender for Cloud

    Onboarding might take up to 1 hour.

1. To verify installation of Defender for Endpoint on a Linux machine, run the following shell command on your machines:

    `mdatp health`

    If Microsoft Defender for Endpoint is installed, you'll see its health status:

    `healthy : true`

    `licensed: true`

    In addition, in the Azure portal you'll see a new Azure extension on your machines called `MDE.Linux`.

##### Enable for multiple subscriptions in the Azure portal dashboard

If one or more of your subscriptions don't have Endpoint protections enabled for Linux machines, you'll see an insight panel in the Defender for Cloud dashboard. The insight panel tells you about subscriptions that have Defender for Endpoint integration enabled for Windows machines, but not for Linux machines. You can use the insight panel to see the affected subscriptions with the number of affected resources in each subscription. Subscriptions that don't have Linux machines show no affected resources. You can then select the subscriptions to enable endpoint protection for Linux integration.

After you select **Enable** in the insight panel, Defender for Cloud:

- Automatically onboards your Linux machines to Defender for Endpoint in the selected subscriptions.
- Detects any previous installations of Defender for Endpoint and reconfigure them to integrate with Defender for Cloud.

Use the [Defender for Endpoint status workbook](https://aka.ms/MDEStatus) to verify installation and deployment status of Defender for Endpoint on a Linux machine.

##### Enable for multiple subscriptions with a PowerShell script

Use our [PowerShell script](https://github.com/Azure/Microsoft-Defender-for-Cloud/tree/main/Powershell%20scripts/Enable%20MDE%20Integration%20for%20Linux) from the Defender for Cloud GitHub repository to enable endpoint protection on Linux machines that are in multiple subscriptions.

### Enable the MDE unified solution at scale

You can also enable the MDE unified solution at scale through the supplied REST API version 2022-05-01. For full details, see the [API documentation](/rest/api/defenderforcloud/settings/update?tabs=HTTP).

Here's an example request body for the PUT request to enable the MDE unified solution:

URI: `https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.Security/settings/WDATP_UNIFIED_SOLUTION?api-version=2022-05-01`

```json
{
    "name": "WDATP_UNIFIED_SOLUTION",
    "type": "Microsoft.Security/settings",
    "kind": "DataExportSettings",
    "properties": {
        "enabled": true
    }
}
```

## Track MDE deployment status

You can use the [Defender for Endpoint deployment status workbook](https://github.com/Azure/Microsoft-Defender-for-Cloud/tree/main/Workbooks/Defender%20for%20Endpoint%20Deployment%20Status) to track the MDE deployment status on your Azure VMs and non-Azure machines that are connected via Azure Arc. The interactive workbook provides an overview of machines in your environment showing their Microsoft Defender for Endpoint extension deployment status.

## Access the Microsoft Defender for Endpoint portal

1. Ensure the user account has the necessary permissions. Learn more in [Assign user access to Microsoft Defender Security Center](/microsoft-365/security/defender-endpoint/assign-portal-access).

1. Check whether you have a proxy or firewall that is blocking anonymous traffic. The Defender for Endpoint sensor connects from the system context, so anonymous traffic must be permitted. To ensure unhindered access to the Defender for Endpoint portal, follow the instructions in [Enable access to service URLs in the proxy server](/microsoft-365/security/defender-endpoint/configure-proxy-internet#enable-access-to-microsoft-defender-for-endpoint-service-urls-in-the-proxy-server).

1. Open the [Microsoft 365 Defender portal](https://security.microsoft.com/). Learn about [Microsoft Defender for Endpoint in Microsoft 365 Defender](/microsoft-365/security/defender/microsoft-365-security-center-mde).

## Send a test alert

To generate a benign test alert from Defender for Endpoint, select the tab for the relevant operating system of your endpoint:

- [Test on Windows](#test-on-windows)

- [Test on Linux](#test-on-linux)

### Test on Windows

For endpoints running Windows:

1. Create a folder 'C:\test-MDATP-test'.
1. Use Remote Desktop to access your machine.
1. Open a command-line window.
1. At the prompt, copy and run the following command. The command prompt window will close automatically.

    ```powershell
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```
    :::image type="content" source="./media/integration-defender-for-endpoint/generate-edr-alert.png" alt-text="A command prompt window with the command to generate a test alert.":::

    If the command is successful, you'll see a new alert on the workload protection dashboard and the Microsoft Defender for Endpoint portal. This alert might take a few minutes to appear.
1. To review the alert in Defender for Cloud, go to **Security alerts** > **Suspicious PowerShell CommandLine**.
1. From the investigation window, select the link to go to the Microsoft Defender for Endpoint portal.

    > [!TIP]
    > The alert is triggered with **Informational** severity.

### Test on Linux

For endpoints running Linux:

1. Download the test alert tool from: <https://aka.ms/LinuxDIY>
1. Extract the contents of the zip file and execute this shell script:

    `./mde_linux_edr_diy`

    If the command is successful, you'll see a new alert on the workload protection dashboard and the Microsoft Defender for Endpoint portal. This alert might take a few minutes to appear.

1. To review the alert in Defender for Cloud, go to **Security alerts** > **Enumeration of files with sensitive data**.
1. From the investigation window, select the link to go to the Microsoft Defender for Endpoint portal.

    > [!TIP]
    > The alert is triggered with **Low** severity.

## Remove Defender for Endpoint from a machine

To remove the Defender for Endpoint solution from your machines:

1. Disable the integration:
    1. From Defender for Cloud's menu, select **Environment settings** and select the subscription with the relevant machines.
    1. Open **Integrations** and clear the checkbox for **Allow Microsoft Defender for Endpoint to access my data**.
    1. Select **Save**.

1. Remove the MDE.Windows/MDE.Linux extension from the machine.

1. Follow the steps in [Offboard devices from the Microsoft Defender for Endpoint service](/microsoft-365/security/defender-endpoint/offboard-machines) from the Defender for Endpoint documentation.

## FAQ - Microsoft Defender for Cloud integration with Microsoft Defender for Endpoint

- [What's this "MDE.Windows" / "MDE.Linux" extension running on my machine?](#whats-this-mdewindows--mdelinux-extension-running-on-my-machine)
- [What are the licensing requirements for Microsoft Defender for Endpoint?](#what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint)
- [Do I need to buy a separate anti-malware solution to protect my machines?](#do-i-need-to-buy-a-separate-anti-malware-solution-to-protect-my-machines)
- [If I already have a license for Microsoft Defender for Endpoint, can I get a discount for Microsoft Defender for Servers?](#if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-microsoft-defender-for-servers)
- [How do I switch from a third-party EDR tool?](#how-do-i-switch-from-a-third-party-edr-tool)

### What's this "MDE.Windows" / "MDE.Linux" extension running on my machine?

In the past, Microsoft Defender for Endpoint was provisioned by the Log Analytics agent. When [we expanded support to include Windows Server 2019](release-notes-archive.md#microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-on-windows-virtual-desktop-released-for-general-availability-ga) and Linux, we also added an extension to perform the automatic onboarding.

Defender for Cloud automatically deploys the extension to machines running:

- Windows Server 2019 and Windows Server 2022
- Windows Server 2012 R2 and 2016 if [MDE Unified Solution integration](#enable-the-integration) is enabled
- Windows 10 on Azure Virtual Desktop.
- Other versions of Windows Server if Defender for Cloud doesn't recognize the OS version (for example, when a custom VM image is used). In this case, Microsoft Defender for Endpoint is still provisioned by the Log Analytics agent.
- Linux.

> [!IMPORTANT]
> If you delete the MDE.Windows/MDE.Linux extension, it will not remove Microsoft Defender for Endpoint. to 'offboard', see [Offboard Windows servers.](/microsoft-365/security/defender-endpoint/configure-server-endpoints).

### I enabled the solution but the `MDE.Windows`/`MDE.Linux` extension isn't showing on my machine

If you enabled the integration, but still don't see the extension running on your machines:

1. You need to wait at least 12 hours to be sure there's an issue to investigate.
1. If after 12 hours you still don't see the extension running on your machines, check that you've met [Prerequisites](#prerequisites) for the integration.
1. Ensure you've enabled the [Microsoft Defender for Servers](defender-for-servers-introduction.md) plan for the subscriptions related to the machines you're investigating.
1. If you've moved your Azure subscription between Azure tenants, some manual preparatory steps are required before Defender for Cloud will deploy Defender for Endpoint. For full details, [contact Microsoft support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

### What are the licensing requirements for Microsoft Defender for Endpoint?

Licenses for Defender for Endpoint for servers are included with **Microsoft Defender for Servers**.

### Do I need to buy a separate anti-malware solution to protect my machines?

No. With MDE integration in Defender for Servers, you'll also get malware protection on your machines.
- On Windows Server 2012 R2 with MDE unified solution integration enabled, Defender for Servers will deploy [Microsoft Defender Antivirus](/microsoft-365/security/defender-endpoint/microsoft-defender-antivirus-windows) in *active mode*.
- On newer Windows Server operating systems, Microsoft Defender Antivirus is part of the operating system and will be enabled in *active mode*.
- On Linux, Defender for Servers will deploy MDE including the anti-malware component, and set the component in *passive mode*.

### If I already have a license for Microsoft Defender for Endpoint, can I get a discount for Microsoft Defender for Servers?

If you already have a license for **Microsoft Defender for Endpoint for Servers** , you won't pay for that part of your [Microsoft Defender for Servers Plan 2](plan-defender-for-servers-select-plan.md#plan-features) license. Learn more about [the Microsoft 365 license](/microsoft-365/security/defender-endpoint/minimum-requirements#licensing-requirements).

To request your discount, [contact Defender for Cloud's support team](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). You'll need to provide the relevant workspace ID, region, and number of Microsoft Defender for Endpoint for servers licenses applied for machines in the given workspace.

The discount will be effective starting from the approval date, and won't take place retroactively.

### How do I switch from a third-party EDR tool?

Full instructions for switching from a non-Microsoft endpoint solution are available in the Microsoft Defender for Endpoint documentation: [Migration overview](/windows/security/threat-protection/microsoft-defender-atp/switch-to-microsoft-defender-migration).

### Which Microsoft Defender for Endpoint plan is supported in Defender for Servers?

Defender for Servers Plan 1 and Plan 2 provides the capabilities of [Microsoft Defender for Endpoint Plan 2](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint).

## Next steps

- [Platforms and features supported by Microsoft Defender for Cloud](security-center-os-coverage.md)
- [Learn how recommendations help you protect your Azure resources](review-security-recommendations.md)

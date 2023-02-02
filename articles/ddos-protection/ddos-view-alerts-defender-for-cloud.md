---
title: 'View Azure DDoS Protection alerts in Microsoft Defender for Cloud'
description: Learn how to view DDoS protection alerts in Microsoft Defender for Cloud.
services: ddos-protection
documentationcenter: na
author: AbdullahBell
ms.service: ddos-protection
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2023
ms.author: abell
---

# View Azure DDoS Protection alerts in Microsoft Defender for Cloud

Microsoft Defender for Cloud provides a list of [security alerts](../security-center/security-center-managing-and-responding-alerts.md), with information to help investigate and remediate problems. With this feature, you get a unified view of alerts, including DDoS attack-related alerts and the actions taken to mitigate the attack in near-time.
There are two specific alerts that you'll see for any DDoS attack detection and mitigation:

- **DDoS Attack detected for Public IP**: This alert is generated when the DDoS protection service detects that one of your public IP addresses is the target of a DDoS attack.
- **DDoS Attack mitigated for Public IP**: This alert is generated when an attack on the public IP address has been mitigated.
To view the alerts, open **Defender for Cloud** in the Azure portal and select **Security alerts**. Under **Threat Protection**, select **Security alerts**. The following screenshot shows an example of the DDoS attack alerts.

    :::image type="content" source="./media/manage-ddos-protection/ddos-alert-asc.png" alt-text="Screenshot of DDoS Alert in Microsoft Defender for Cloud." lightbox="./media/manage-ddos-protection/ddos-alert-asc.png":::

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [DDoS Network Protection](manage-ddos-protection.md) must be enabled on a virtual network or [DDoS IP Protection (Preview)](manage-ddos-protection-powershell-ip.md) must be enabled on a public IP address. 

## View alerts in Microsoft Defender for Cloud

1. Sign in to the [Azure portal](https://portal.azure.com/).
1. In the search box at the top of the portal, enter **Microsoft Defender for Cloud**. Select **Microsoft Defender for Cloud** in the search results.
1. Under *General* in the side tab, select **Security alerts**. To filter the alerts list, select your subscription, or any of the relevant filters. You can optionally add filters with the **Add filter** option.

    :::image type="content" source="./media/manage-ddos-protection/ddos-protection-security-alerts.png" alt-text="Screenshot of Security alert in Microsoft Defender for Cloud.":::
 
The alerts include general information about the public IP address that’s under attack, geo and threat intelligence information, and remediation steps.

## Next steps

In this How-To, you learned how to view alerts in Microsoft Defender for Cloud.

To learn how to test and simulate a DDoS attack, see the simulation testing guide:

> [!div class="nextstepaction"]
> [Test through simulations](test-through-simulations.md)
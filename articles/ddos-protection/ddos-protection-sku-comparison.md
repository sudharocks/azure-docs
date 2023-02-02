---
title: 'About Azure DDoS Protection SKU Comparison'
description: Learn about the available SKUs for Azure DDoS Protection.
author: AbdullahBell
ms.author: Abell
ms.service: ddos-protection
ms.topic: conceptual 
ms.date: 01/17/2023
ms.custom: template-concept, ignite-2022
---


# About Azure DDoS Protection SKU Comparison


The sections in this article discuss the resources and settings of Azure DDoS Protection.

## DDoS Network Protection

Azure DDoS Network Protection, combined with application design best practices, provides enhanced DDoS mitigation features to defend against DDoS attacks. It's automatically tuned to help protect your specific Azure resources in a virtual network. For more information about enabling DDoS Network Protection, see [Quickstart: Create and configure Azure DDoS Network Protection using the Azure portal](manage-ddos-protection.md).

## DDoS IP Protection Preview

 DDoS IP Protection is a pay-per-protected IP model. DDoS IP Protection contains the same core engineering features as DDoS Network Protection, but will differ in the following value-added services: DDoS rapid response support, cost protection, and discounts on WAF. For more information about enabling DDoS IP Protection, see [Quickstart: Create and configure Azure DDoS IP Protection using Azure PowerShell](manage-ddos-protection-powershell-ip.md).

> [!NOTE]
> DDoS IP Protection is currently only available in Azure Preview PowerShell.

## SKUs

Azure DDoS Protection supports two SKU Types, DDoS IP Protection and DDoS Network Protection. The SKU is configured in the Azure portal during the workflow when you configure Azure DDoS Protection.

The following table shows features and corresponding SKUs.

| Feature | DDoS IP Protection | DDoS Network Protection |
|---|---|---|
| Protects Public IP Standard SKU | Yes | Yes |
| Protects Public IP Basic SKU | No | Yes |
| Active traffic monitoring & always on detection |  Yes| Yes |
| L3/L4 Automatic attack mitigation  | Yes | Yes |
| Automatic attack mitigation | Yes | Yes |
| Application based mitigation policies | Yes| Yes |
| Metrics & alerts | Yes | Yes |
| Mitigation reports | Yes | Yes |
| Mitigation flow logs| Yes| Yes |
| Mitigation policies tuned to customers application | Yes| Yes |
| Integration with Firewall Manager | Yes | Yes |
| Azure Sentinel data connector and workbook | Yes | Yes |
| Protection of resources across subscriptions in a tenant   | Yes | Yes |
| Public IP Standard SKU protection | Yes | Yes |
| Public IP Basic SKU protection | No | Yes |
| DDoS rapid response support | Not available | Yes |
| Cost protection | Not available  | Yes |
| WAF discount | Not available | Yes |
| Price | Per protected IP | Per 100 protected IP addresses |

>[!Note]
>At no additional cost, Azure DDoS infrastructure protection protects every Azure service that uses public IPv4 and IPv6 addresses. This DDoS protection service helps to protect all Azure services, including platform as a service (PaaS) services such as Azure DNS. For more information on supported PaaS services, see [DDoS Protection reference architectures](ddos-protection-reference-architectures.md). Azure DDoS infrastructure protection requires no user configuration or application changes. Azure provides continuous protection against DDoS attacks. DDoS protection does not store customer data.


## Next steps

* [Quickstart: Create an Azure DDoS Protection Plan](manage-ddos-protection.md)
* [Azure DDoS Protection features](ddos-protection-features.md)

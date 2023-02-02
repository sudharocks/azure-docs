---
title: What's new in Microsoft Sentinel
description: This article describes new features in Microsoft Sentinel from the past few months.
author: yelevin
ms.author: yelevin
ms.topic: conceptual
ms.date: 01/31/2023
---

# What's new in Microsoft Sentinel

This article lists recent features added for Microsoft Sentinel, and new features in related services that provide an enhanced user experience in Microsoft Sentinel.

The listed features were released in the last three months. For information about earlier features delivered, see our [Tech Community blogs](https://techcommunity.microsoft.com/t5/azure-sentinel/bg-p/AzureSentinelBlog/label-name/What's%20New).

See these [important announcements](#announcements) about recent changes to features and services.

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

## February 2023

- [Microsoft 365 Defender data connector is now generally available](#microsoft-365-defender-data-connector-is-now-generally-available)
- [Advanced scheduling for analytics rules (Preview)](#advanced-scheduling-for-analytics-rules-preview)

### Microsoft 365 Defender data connector is now generally available

Microsoft 365 Defender incidents, alerts, and raw event data can be ingested into Microsoft Sentinel using this connector. It also enables the bi-directional synchronization of incidents between Microsoft 365 Defender and Microsoft Sentinel. This integration allows you to manage all of your incidents in Microsoft Sentinel, while taking advantage of Microsoft 365 Defender's specialized tools and capabilities to investigate those incidents that originated in Microsoft 365.

- Learn more about [Microsoft 365 Defender integration with Microsoft Sentinel](microsoft-365-defender-sentinel-integration.md).
- Learn how to [connect Microsoft 365 Defender to Microsoft Sentinel](connect-microsoft-365-defender.md).

### Advanced scheduling for analytics rules (Preview)

To give you more flexibility in scheduling your analytics rule execution times and to help you avoid potential conflicts, Microsoft Sentinel now allows you to determine when newly created analytics rules will run for the first time. The default behavior is as it has been: for them to run immediately upon creation.

[Learn more about advanced scheduling](detect-threats-custom.md#query-scheduling-and-alert-threshold).

## January 2023

- [New incident investigation experience (Preview)](#new-incident-investigation-experience-preview)
- [Monitor SAP system health (Preview)](#monitor-sap-system-health-and-role-preview)
- [Microsoft Purview Information Protection connector (Preview)](#microsoft-purview-information-protection-connector-preview)

### New incident investigation experience (Preview)

SOC analysts need to understand the full scope of an attack as fast as possible to respond effectively. 

While triaging, investigating, and responding to a security incident, analysts require quick and seamless access to many pieces of information, actions, and tools. This access should optimally be within the incident investigation environment, with an absolute minimum of pivoting to other pages, products, or services&mdash;for example, to find Azure AD info or the geo-location of an IP, edit a bookmark, or add an entity to threat intelligence.

**Microsoft Sentinel now offers a new incident investigation experience**. The new incident page design, along with many new features for investigation, response, and incident management, offers the analyst the information and tools necessary to understand the incident and the scope of breach, while making navigation easy and context switching less frequent. New features include, among others, top insights, a new activity log for incident audits and a Log Analytics query window to investigate logs.

Learn more about the new investigation experience:
- [Understand Microsoft Sentinel's incident investigation and case management capabilities](incident-investigation.md)
- [Navigate and investigate incidents in Microsoft Sentinel](investigate-incidents.md)

### Monitor SAP system health and role (Preview)

To ensure proper functioning and performance of your SAP systems, you can now use the SAP data connector page to [monitor information about the health of your SAP systems](monitor-sap-system-health.md) and the status of the SAP roles for the system. You can also use an alert rule template to get information about the health of the SAP agent's data collection.

### Microsoft Purview Information Protection connector (Preview)

With the new [Microsoft Purview Information Protection connector](connect-microsoft-purview.md), you can stream data from Microsoft Purview Information Protection (formerly Microsoft Information Protection or MIP) to Microsoft Sentinel. You can use the data ingested from the Microsoft Purview labeling clients and scanners to track, analyze, report on the data, and use it for compliance purposes.

> [!IMPORTANT]
> This connector replaces the Azure Information Protection (AIP) data connector, aligned with the retirement of the AIP analytics and audit logs public preview as of **March 31, 2023**.

The new connector streams audit logs into the standardized 
`MicrosoftPurviewInformationProtection` table, which has been adjusted to enhance the deprecated schema used by AIP, with more fields and easier access to parameters. Data is gathered through the [Office Management API](/office/office-365-management-api/office-365-management-activity-api-schema), which uses a structured schema. Review the list of supported [audit log record types and activities](microsoft-purview-record-types-activities.md).

## December 2022

- [Create and run playbooks on entities on-demand (Preview)](#create-and-run-playbooks-on-entities-on-demand-preview)
- [Customize more alert properties (Preview)](#customize-more-alert-properties-preview)

### Create and run playbooks on entities on-demand (Preview)

SOC analysts can now take immediate action on a particular entity representing a threat actor, while in the middle of investigating an incident or hunting for threats, without leaving those contexts or having to pivot to other screens or apps. 

Similarly, SOC engineers can now encapsulate a series of automated actions in workflows that run on a specific entity, so that analysts can use these workflows in the scenarios above.

These improvements for SOC efficiency and productivity are thanks to the **new entity trigger for playbooks**.

- Learn more about [running playbooks on entities on-demand](respond-threats-during-investigation.md).
- Learn more about [creating playbooks based on the entity trigger](tutorial-respond-threats-playbook.md#create-a-playbook).

### Customize more alert properties (Preview)

Alerts generated by a given analytics rule - and all incidents created as a result - inherit the name, description, severity, and tactics defined in the rule, without regard to the particular content of a specific instance of the alert.

You've already been able to use the **alert details** feature to override these four default properties of alerts; now there are **nine more alert properties** that can be customized to override their defaults.

See which ones, and learn how to use the updated mechanism, in [Customize alert details in Microsoft Sentinel](customize-alert-details.md).

## November 2022

- [Use Incident tasks to manage incident workflow (Preview)](#use-incident-tasks-to-manage-incident-workflow-preview)
- [Common Event Format (CEF) via AMA (Preview)](#common-event-format-cef-via-ama-preview)
- [Monitor the health of automation rules and playbooks](#monitor-the-health-of-automation-rules-and-playbooks)
- [Updated Microsoft Sentinel Logstash plugin](#updated-microsoft-sentinel-logstash-plugin)

### Use Incident tasks to manage incident workflow (Preview)

SecOps analysts are expected to perform a list of steps, or tasks, in the process of triaging, investigating, or remediating an incident. Standardizing and formalizing the list of tasks can help keep your SOC running smoothly, ensuring the same requirements apply to all analysts.

SOC managers, automation engineers, and senior analysts can use Microsoft Sentinel's automation capabilities to generate lists of tasks that will apply across groups of incidents based on their content, ensuring that front-line analysts apply the same standards of care across the board and don't miss any critical steps.

- Learn more about [incident tasks](incident-tasks.md).
- Learn how analysts can [use tasks to handle incident workflow](work-with-tasks.md).
- Learn how to add tasks to groups of incidents automatically using [automation rules](create-tasks-automation-rule.md) or [playbooks](create-tasks-playbook.md).

### Common Event Format (CEF) via AMA (Preview)

The [Common Event Format (CEF) via AMA](connect-cef-ama.md) connector allows you to quickly filter and upload logs over CEF from multiple on-premises appliances to Microsoft Sentinel via the Azure Monitor Agent (AMA).

The AMA supports Data Collection Rules (DCRs), which you can use to filter the logs before ingestion, for quicker upload, efficient analysis, and querying.

### Monitor the health of automation rules and playbooks

To ensure proper functioning and performance of your security orchestration, automation, and response operations in your Microsoft Sentinel service, keep track of the health of your automation rules and playbooks by monitoring their execution logs.

Set up notifications of health events for relevant stakeholders, who can then take action. For example, define and send email or Microsoft Teams messages, create new tickets in your ticketing system, and so on.

- Learn what [health monitoring in Microsoft Sentinel](health-audit.md) can do for you.
- [Turn on health monitoring](enable-monitoring.md) in Microsoft Sentinel.
- Monitor the health of your [automation rules and playbooks](monitor-automation-health.md).
- See more information about the [*SentinelHealth* table schema](health-table-reference.md).

### Updated Microsoft Sentinel Logstash plugin

A [new version of the Microsoft Sentinel Logstash plugin](connect-logstash-data-connection-rules.md) leverages the new Azure Monitor Data Collection Rules (DCR) based Logs Ingestion API. The new plugin: 

- Provides data transformation capabilities like filtering, masking, and enrichment. 
- Allows full control over the output schema, including configuration of the column names and types. 
- Can forward logs from external data sources into both custom tables and standard tables. 
- Provides performance improvements, compression, and better telemetry and error handling.

## Announcements

- [Microsoft 365 Defender now integrates Azure Active Directory Identity Protection (AADIP)](#microsoft-365-defender-now-integrates-azure-active-directory-identity-protection-aadip)
- [Account enrichment fields removed from Azure AD Identity Protection connector](#account-enrichment-fields-removed-from-azure-ad-identity-protection-connector)
- [Name fields removed from UEBA UserPeerAnalytics table](#name-fields-removed-from-ueba-userpeeranalytics-table)

### Microsoft 365 Defender now integrates Azure Active Directory Identity Protection (AADIP)

As of **October 24, 2022**, [Microsoft 365 Defender](/microsoft-365/security/defender/) integrates [Azure Active Directory Identity Protection (AADIP)](../active-directory/identity-protection/index.yml) alerts and incidents. Customers can choose between three levels of integration:

- **Show high-impact alerts only (Default)** includes only alerts about known malicious or highly suspicious activities that might require attention. These alerts are  chosen by Microsoft security researchers and are mostly of Medium and High severities.
- **Show all alerts** includes all AADIP alerts, including activity that might not be unwanted or malicious.
- **Turn off all alerts** disables any AADIP alerts from appearing in your Microsoft 365 Defender incidents.

Microsoft Sentinel customers (who are also AADIP subscribers) with [Microsoft 365 Defender integration](microsoft-365-defender-sentinel-integration.md) enabled now automatically receive AADIP alerts and incidents in their Microsoft Sentinel incidents queue. Depending on your configuration, this may affect you as follows:

- If you already have your AADIP connector enabled in Microsoft Sentinel, and you've enabled incident creation, you may receive duplicate incidents. To avoid this, you have a few choices, listed here in descending order of preference:

    | Preference | Action in Microsoft 365 Defender | Action in Microsoft Sentinel |
    | - | - | - |
    | **1** | Keep the default AADIP integration of **Show high-impact alerts only**. | Disable any [**Microsoft Security** analytics rules](detect-threats-built-in.md) that create incidents from AADIP alerts. |
    | **2** | Choose the **Show all alerts** AADIP integration. | Create automation rules to automatically close incidents with unwanted alerts.<br><br>Disable any [**Microsoft Security** analytics rules](detect-threats-built-in.md) that create incidents from AADIP alerts. |
    | **3** | Don't use Microsoft 365 Defender for AADIP alerts:<br>Choose the **Turn off all alerts** option for AADIP integration. | Leave enabled those [**Microsoft Security** analytics rules](detect-threats-built-in.md) that create incidents from AADIP alerts. |

- If you don't have your [AADIP connector](data-connectors-reference.md#azure-active-directory-identity-protection) enabled, you must enable it. Be sure **not** to enable incident creation on the connector page. If you don't enable the connector, you may receive AADIP incidents without any data in them.

- If you're first enabling your Microsoft 365 Defender connector now, the AADIP connection was made automatically behind the scenes. You won't need to do anything else.

### Account enrichment fields removed from Azure AD Identity Protection connector

As of **September 30, 2022**, alerts coming from the **Azure Active Directory Identity Protection connector** no longer contain the following fields:

- CompromisedEntity
- ExtendedProperties["User Account"]
- ExtendedProperties["User Name”]

We are working to adapt Microsoft Sentinel's built-in queries and other operations affected by this change to look up these values in other ways (using the *IdentityInfo* table).

In the meantime, or if you've built any custom queries or rules directly referencing these fields, you'll need another way to get this information. Use the following two-step process to have your queries look up these values in the *IdentityInfo* table:

1. If you haven't already, **enable the UEBA solution** to sync the *IdentityInfo* table with your Azure AD logs. Follow the instructions in [this document](enable-entity-behavior-analytics.md).  
(If you don't intend to use UEBA in general, you can ignore the last instruction  about selecting data sources on which to enable entity behavior analytics.)

1. Incorporate the query below in your existing queries or rules to look up this data by joining the *SecurityAlert* table with the *IdentityInfo* table.

    ```kusto
    SecurityAlert
    | where TimeGenerated > ago(7d)
    | where ProductName == "Azure Active Directory Identity Protection"
    | mv-expand Entity = todynamic(Entities)
    | where Entity.Type == "account"
    | extend AadTenantId = tostring(Entity.AadTenantId)
    | extend AadUserId = tostring(Entity.AadUserId)
    | join kind=inner (
        IdentityInfo
        | where TimeGenerated > ago(14d)
        | distinct AccountTenantId, AccountObjectId, AccountUPN, AccountDisplayName
        | extend UserAccount = AccountUPN
        | extend UserName = AccountDisplayName
        | where isnotempty(AccountDisplayName) and isnotempty(UserAccount)
        | project AccountTenantId, AccountObjectId, UserAccount, UserName
        )
        on
        $left.AadTenantId == $right.AccountTenantId,
        $left.AadUserId == $right.AccountObjectId
    | extend CompromisedEntity = iff(CompromisedEntity == "N/A" or isempty(CompromisedEntity), UserAccount, CompromisedEntity)
    | project-away AadTenantId, AadUserId, AccountTenantId, AccountObjectId
    ```

For information on looking up data to replace enrichment fields removed from the UEBA UserPeerAnalytics table, See [Name fields removed from UEBA UserPeerAnalytics table](#name-fields-removed-from-ueba-userpeeranalytics-table) for a sample query.

### Name fields removed from UEBA UserPeerAnalytics table

As of **September 30, 2022**, the UEBA engine no longer performs automatic lookups of user IDs and resolves them into names. This change resulted in the removal of four name fields from the *UserPeerAnalytics* table: 

- UserName
- UserPrincipalName
- PeerUserName
- PeerUserPrincipalName 

The corresponding ID fields remain part of the table, and any built-in queries and other operations execute the appropriate name lookups in other ways (using the IdentityInfo table), so you shouldn’t be affected by this change in nearly all circumstances. 

The only exception to this is if you’ve built custom queries or rules directly referencing any of these name fields. In this scenario, you can incorporate the following lookup queries into your own, so you can access the values that would have been in these name fields. 

The following query resolves **user** and **peer identifier fields**: 

```kusto
UserPeerAnalytics 
| where TimeGenerated > ago(24h) 
// join to resolve user identifier fields 
| join kind=inner ( 
    IdentityInfo  
    | where TimeGenerated > ago(14d) 
    | distinct AccountTenantId, AccountObjectId, AccountUPN, AccountDisplayName 
    | extend UserPrincipalNameIdentityInfo = AccountUPN 
    | extend UserNameIdentityInfo = AccountDisplayName 
    | project AccountTenantId, AccountObjectId, UserPrincipalNameIdentityInfo, UserNameIdentityInfo 
) on $left.AADTenantId == $right.AccountTenantId, $left.UserId == $right.AccountObjectId 
// join to resolve peer identifier fields 
| join kind=inner ( 
    IdentityInfo  
    | where TimeGenerated > ago(14d) 
    | distinct AccountTenantId, AccountObjectId, AccountUPN, AccountDisplayName 
    | extend PeerUserPrincipalNameIdentityInfo = AccountUPN 
    | extend PeerUserNameIdentityInfo = AccountDisplayName 
    | project AccountTenantId, AccountObjectId, PeerUserPrincipalNameIdentityInfo, PeerUserNameIdentityInfo 
) on $left.AADTenantId == $right.AccountTenantId, $left.PeerUserId == $right.AccountObjectId 
```
If your original query referenced the user or peer names (not just their IDs), substitute this query in its entirety for the table name (“UserPeerAnalytics”) in your original query.

## Next steps

> [!div class="nextstepaction"]
>[On-board Azure Sentinel](quickstart-onboard.md)

> [!div class="nextstepaction"]
>[Get visibility into alerts](get-visibility.md)

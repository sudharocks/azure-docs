---
title: Syslog collection with Container Insights
description: This article describes how to collect Syslog from AKS nodes using Container insights.
ms.topic: conceptual
ms.date: 01/31/2023
ms.reviewer: damendo
---

# Syslog collection with Container Insights (preview)

Container Insights offers the ability to collect Syslog events from Linux nodes in your [Azure Kubernetes Service (AKS)](../../aks/intro-kubernetes.md) clusters. Customers can use Syslog for monitoring security and health events, typically by ingesting syslog into SIEM systems like [Microsoft Sentinel](https://azure.microsoft.com/products/microsoft-sentinel/#overview).  

> [!IMPORTANT]
> Syslog collection with Container Insights is a preview feature. Preview features are available on a self-service, opt-in basis. Previews are provided "as is" and "as available," and they're excluded from the service-level agreements and limited warranty. Previews are partially covered by customer support on a best-effort basis. As such, these features aren't meant for production use.

## Prerequisites 

- You will need to have managed identity authentication enabled on your cluster. To enable, see [migrate your AKS cluster to managed identity authentication](container-insights-enable-existing-clusters.md?tabs=azure-cli#migrate-to-managed-identity-authentication). Note: This which will create a Data Collection Rule (DCR) named `MSCI-<WorkspaceRegion>-<ClusterName>` 
- Minimum versions of Azure components
  - **Azure CLI**: Minimum version required for Azure CLI is [2.44.1 (link to release notes)](/cli/azure/release-notes-azure-cli#january-11-2023). See [How to update the Azure CLI](/cli/azure/update-azure-cli) for upgrade instructions. 
  - **Azure CLI AKS-Preview Extension**: Minimum version required for AKS-Preview Azure CLI extension is [ 0.5.125 (link to release notes)](https://github.com/Azure/azure-cli-extensions/blob/main/src/aks-preview/HISTORY.rst#05125). See [How to update extensions](/cli/azure/azure-cli-extensions-overview#how-to-update-extensions) for upgrade guidance. 
  - **Linux image version**: Minimum version for AKS node linux image is 2022.11.01. See [Upgrade Azure Kubernetes Service (AKS) node images](https://learn.microsoft.com/azure/aks/node-image-upgrade) for upgrade help. 

## How to enable Syslog
  
Use the following command in Azure CLI to enable syslog collection when you create a new AKS cluster.

```azurecli
az aks create -g syslog-rg -n new-cluster --enable-managed-identity --node-count 1 --enable-addons monitoring --enable-msi-auth-for-monitoring --enable-syslog --generate-ssh-key
```
  
Use the following command in Azure CLI to enable syslog collection on an existing AKS cluster.

```azurecli
az aks enable-addons -a monitoring --enable-msi-auth-for-monitoring --enable-syslog -g syslog-rg -n existing-cluster
```


## How to access Syslog data
 
Syslog data is stored in the [Syslog](/azure/azure-monitor/reference/tables/syslog) table in your Log Analytics workspace. You can create your own [log queries](../logs/log-query-overview.md) in [Log Analytics](../logs/log-analytics-overview.md) to analyze this data or use any of the [prebuilt queries](../logs/log-query-overview.md).

:::image type="content" source="media/container-insights-syslog/azmon-3.png" lightbox="media/container-insights-syslog/azmon-3.png" alt-text="Screenshot of Syslog query loaded in the query editor in the Azure Monitor Portal UI." border="false":::    

You can open Log Analytics from the **Logs** menu in the **Monitor** menu to access Syslog data for all clusters or from the AKs cluster's menu to access Syslog data for only that cluster.
 
:::image type="content" source="media/container-insights-syslog/aks-4.png" lightbox="media/container-insights-syslog/aks-4.png" alt-text="Screenshot of Query editor with Syslog query." border="false":::
  
### Sample queries
  
The following table provides different examples of log queries that retrieve Syslog records.

| Query | Description |
|:--- |:--- |
| `Syslog` |All Syslogs |
| `Syslog | where SeverityLevel == "error"` |All Syslog records with severity of error |
| `Syslog | summarize AggregatedValue = count() by Computer` |Count of Syslog records by computer |
| `Syslog | summarize AggregatedValue = count() by Facility` |Count of Syslog records by facility |  

## Editing your Syslog collection settings

To modify the configuration for your Syslog collection, you modify the [data collection rule (DCR)](../essentials/data-collection-rule-overview.md) that was created when you enabled it. 

Select **Data Collection Rules** from the **Monitor** menu in the Azure portal. 

:::image type="content" source="media/container-insights-syslog/dcr-1.png" lightbox="media/container-insights-syslog/dcr-1.png" alt-text="Screenshot of Data Collection Rules tab in the Azure Monitor portal UI." border="false":::

Select your DCR and then **View data sources**. Select the **Linux Syslog** data source to view the Syslog collection details.
>[!NOTE]
> A DCR is created automatically when you enable syslog. The DCR follows the naming convention `MSCI-<WorkspaceRegion>-<ClusterName>`.

:::image type="content" source="media/container-insights-syslog/dcr-3.png" lightbox="media/container-insights-syslog/dcr-3.png" alt-text="Screenshot of Data Sources tab for Syslog data collection rule." border="false":::

Select the minimum log level for each facility that you want to collect.

:::image type="content" source="media/container-insights-syslog/dcr-4.png" lightbox="media/container-insights-syslog/dcr-4.png" alt-text="Screenshot of Configuration panel for Syslog data collection rule." border="false":::


## Known limitations

- **Onboarding**. Syslog collection can only be enabled from command line during public preview. 
- **Container restart data loss**. Agent Container restarts can lead to syslog data loss during public preview. 

## Next steps

- Read more about [Syslog record properties](/azure/azure-monitor/reference/tables/syslog)



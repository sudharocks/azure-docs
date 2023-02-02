---
title: Azure Monitor Logs Dedicated Clusters
description: Customers meeting the minimum commitment tier could use dedicated clusters
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 01/01/2023
ms.custom: devx-track-azurepowershell, devx-track-azurecli
---

# Azure Monitor Logs Dedicated Clusters

Log Analytics Dedicated clusters in Azure Monitor enable advanced capabilities, and higher query utilization, provided to linked Log Analytics workspaces. Clusters require a minimum ingestion commitment of 500 GB per day. You can link and unlink workspaces from a dedicated cluster without any data loss or service interruption. 

Capabilities that require dedicated clusters:

- **[Customer-managed Keys](../logs/customer-managed-keys.md)** - Encrypt the cluster data using keys that are provided and controlled by the customer.
- **[Lockbox](../logs/customer-managed-keys.md#customer-lockbox-preview)** - Control Microsoft support engineers access requests to your data.
- **[Double encryption](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption)** - Protects against a scenario where one of the encryption algorithms or keys may be compromised. In this case, the additional layer of encryption continues to protect your data.
- **[Cross-query optimization](../logs/cross-workspace-query.md)** - Cross-workspace queries run faster when workspaces are on the same cluster.
- **Cost optimization** - Link your workspaces in same region to cluster to get commitment tier discount to all workspaces, even to ones with low ingestion that aren't eligible for commitment tier discount.
- **[Availability zones](../../availability-zones/az-overview.md)** - Protect your data from datacenter failures with zones being separated physically by locations and equipped with independent power, cooling, and networking. The physical separation in zones and independent infrastructure makes an incident far less likely since the workspace can rely on the resources from any of the zones. [Azure Monitor availability zones](./availability-zones.md) covers broader parts of the service and when available in your region, extends your Azure Monitor resiliency automatically. Dedicated clusters are created as Availability zones enabled (`isAvailabilityZonesEnabled`: 'true') by default in supported regions. This setting can’t be altered once created, and can be verified in cluster’s property `isAvailabilityZonesEnabled`. Availability zones clusters are created in the following regions currently, and more regions are added periodically. 

  | Americas | Europe | Middle East | Africa | Asia Pacific |
  |---|---|---|---|---|
  | Brazil South | France Central | UAE North | South Africa North | Australia East |
  | Canada Central | Germany West Central | | | Central India |
  | Central US | North Europe | | | Japan East |
  | East US | Norway East | | | Korea Central |
  | East US 2 | UK South | | | Southeast Asia |
  | South Central US | West Europe | | | East Asia |
  | US Gov Virginia | Sweden Central | | | China North 3 |
  | West US 2 | Switzerland North | | | |
  | West US 3 | | | | |


## Cluster management

Dedicated clusters are managed with an Azure resource that represents Azure Monitor Log clusters. Operations are performed programmatically using [CLI](/cli/azure/monitor/log-analytics/cluster), [PowerShell](/powershell/module/az.operationalinsights) or the [REST](/rest/api/loganalytics/clusters).

Once a cluster is created, workspaces can be linked to it, and new ingested data to them is stored on the cluster. Workspaces can be unlinked from a cluster at any time and new data then stored on shared Log Analytics clusters. The link and unlink operation doesn't affect your queries and access to data before, and after the operation. The Cluster and workspaces must be in the same region.

Operations on the cluster level require Microsoft.OperationalInsights/clusters/write action permission. Linking workspaces to a cluster requires both Microsoft.OperationalInsights/clusters/write and Microsoft.OperationalInsights/workspaces/write actions. Permission could be granted by the Owner or Contributor that have `*/write` action, or by  the Log Analytics Contributor role that have `Microsoft.OperationalInsights/*` action. For more information on Log Analytics permissions, see [Manage access to log data and workspaces in Azure Monitor](./manage-access.md). 


## Cluster pricing model
Log Analytics Dedicated Clusters use a commitment tier pricing model of at least 500 GB/day. Any usage above the tier level will be billed at effective per-GB rate of that commitment tier. See [Azure Monitor Logs pricing details](cost-logs.md#dedicated-clusters) for pricing details for dedicated clusters.

## Create a dedicated cluster

Provide the following properties when creating new dedicated cluster:

- **ClusterName**: Must be unique for the resource group.
- **ResourceGroupName**: You should use a central IT resource group because clusters are usually shared by many teams in the organization. For more design considerations, review Design a Log Analytics workspace configuration(../logs/workspace-design.md).
- **Location**
- **SkuCapacity**: The Commitment Tier (formerly called capacity reservations) can be set to 500, 1000, 2000 or 5000 GB/day. For more information on cluster costs, see [Dedicate clusters](./cost-logs.md#dedicated-clusters). 
- **Managed identity**: Clusters support two [managed identity types](../../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types): System-assigned and User-assigned managed identity, while a single identity can be defined in a cluster depending on your scenario. 
  - System-assigned managed identity is simpler and being generated automatically with the cluster creation when identity `type` is set to "*SystemAssigned*". This identity can be used later to grant storage access to your Key Vault for wrap and unwrap operations.

    *Identity in Cluster's REST Call*
    ```json
    {
      "identity": {
        "type": "SystemAssigned"
        }
    }
    ```
  - User-assigned managed identity lets you configure Customer-managed key at cluster creation, when granting it permissions in your Key Vault before cluster creation.

    *Identity in Cluster's REST Call*
    ```json
    {
    "identity": {
      "type": "UserAssigned",
        "userAssignedIdentities": {
          "subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.ManagedIdentity/UserAssignedIdentities/<cluster-assigned-managed-identity>"
        }
      }  
    }
    ```

The user account that creates the clusters must have the standard Azure resource creation permission: `Microsoft.Resources/deployments/*` and cluster write permission `Microsoft.OperationalInsights/clusters/write` by having in their role assignments this specific action or `Microsoft.OperationalInsights/*` or `*/write`.

After you create your cluster resource, you can edit additional properties such as *sku*, *keyVaultProperties, or *billingType*. See more details below.

You can have up to five active clusters per subscription per region. If the cluster is deleted, it is still reserved for 14 days. You can have up to seven clusters per subscription and region, five active, plus two deleted in past 14 days.

> [!NOTE]
> Cluster creation triggers resource allocation and provisioning. This operation can take a few hours to complete.
> Dedicated cluster is billed once provisioned regardless data ingestion and it's recommended to prepare the deployment to expedite the provisioning and workspaces link to cluster. Verify the following:
> - A list of initial workspace to be linked to cluster is identified
> - You have permissions to subscription intended for the cluster and any workspace to be linked

#### [CLI](#tab/cli)

```azurecli
az account set --subscription "cluster-subscription-id"

az monitor log-analytics cluster create --no-wait --resource-group "resource-group-name" --name "cluster-name" --location "region-name" --sku-capacity "daily-ingestion-gigabyte"

# Wait for job completion when `--no-wait` was used
$clusterResourceId = az monitor log-analytics cluster list --resource-group "resource-group-name" --query "[?contains(name, "cluster-name")].[id]" --output tsv
az resource wait --created --ids $clusterResourceId --include-response-body true
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "cluster-subscription-id"

New-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -Location "region-name" -SkuCapacity "daily-ingestion-gigabyte" -AsJob

# Check when the job is done when `-AsJob` was used
Get-Job -Command "New-AzOperationalInsightsCluster*" | Format-List -Property *
```

#### [REST API](#tab/restapi)

*Call* 

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
Authorization: Bearer <token>
Content-type: application/json

{
  "identity": {
    "type": "systemAssigned"
    },
  "sku": {
    "name": "capacityReservation",
    "Capacity": 500
    },
  "properties": {
    "billingType": "Cluster",
    },
  "location": "<region>",
}
```

*Response*

Should be 202 (Accepted) and a header.

---

### Check cluster provisioning status

The provisioning of the Log Analytics cluster takes a while to complete. Use one of the following methods to check the *ProvisioningState* property. The value is *ProvisioningAccount* while provisioning and *Succeeded* when completed.

#### [CLI](#tab/cli)

```azurecli
az account set --subscription "cluster-subscription-id"

az monitor log-analytics cluster show --resource-group "resource-group-name" --name "cluster-name"
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "cluster-subscription-id"

Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
```
 
#### [REST API](#tab/restapi)

Send a GET request on the cluster resource and look at the *provisioningState* value. The value is *ProvisioningAccount* while provisioning and *Succeeded* when completed.

  ```rest
  GET https://management.azure.com/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name?api-version=2021-06-01
  Authorization: Bearer <token>
  ```

  **Response**

  ```json
  {
    "identity": {
      "type": "SystemAssigned",
      "tenantId": "tenant-id",
      "principalId": "principal-id"
    },
    "sku": {
      "name": "capacityreservation",
      "capacity": 500
    },
    "properties": {
      "provisioningState": "ProvisioningAccount",
      "clusterId": "cluster-id",
      "billingType": "Cluster",
      "lastModifiedDate": "last-modified-date",
      "createdDate": "created-date",
      "isDoubleEncryptionEnabled": false,
      "isAvailabilityZonesEnabled": false,
      "capacityReservationProperties": {
        "lastSkuUpdate": "last-sku-modified-date",
        "minCapacity": 500
      }
    },
    "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
    "name": "cluster-name",
    "type": "Microsoft.OperationalInsights/clusters",
    "location": "cluster-region"
  }
  ```

The *principalId* GUID is generated by the managed identity service at cluster creation.

---


## Link a workspace to a cluster

When a Log Analytics workspace is linked to a dedicated cluster, new data ingested to the workspace, is routed to the cluster while existing data remains in the existing Log Analytics cluster. If the dedicated cluster is configured with customer-managed keys (CMK), new ingested data is encrypted with your key. The system abstracts the data location, you can query data as usual while the system performs cross-cluster queries in the background.

A cluster can be linked to up to 1,000 workspaces. Linked workspaces can be located in the same region as the cluster. A workspace can't be linked to a cluster more than twice a month, to prevent data fragmentation.

You need 'write' permissions to both the workspace and the cluster resource for workspace link operation:

- In the workspace: *Microsoft.OperationalInsights/workspaces/write*
- In the cluster resource: *Microsoft.OperationalInsights/clusters/write*

Other than the billing aspects, configuration of linked workspace remain, including data retention settings.

The workspace and the cluster can be in different subscriptions. It's possible for the workspace and cluster to be in different tenants if Azure Lighthouse is used to map both of them to a single tenant.

Linking a workspace can be performed only after the completion of the Log Analytics cluster provisioning.

> [!WARNING]
> Linking a workspace to a cluster requires syncing multiple backend components and assuring cache hydration. Since this operation may take up to two hours to complete, you should you run it asynchronously.

Use the following commands to link a workspace to a cluster:

#### [CLI](#tab/cli)

```azurecli
# Find cluster resource ID
az account set --subscription "cluster-subscription-id"
$clusterResourceId = az monitor log-analytics cluster list --resource-group "resource-group-name" --query "[?contains(name, "cluster-name")].[id]" --output tsv

# Link workspace
az account set --subscription "workspace-subscription-id"
az monitor log-analytics workspace linked-service create --no-wait --name cluster --resource-group "resource-group-name" --workspace-name "workspace-name" --write-access-resource-id $clusterResourceId

# Wait for job completion when `--no-wait` was used
$workspaceResourceId = az monitor log-analytics workspace list --resource-group "resource-group-name" --query "[?contains(name, "workspace-name")].[id]" --output tsv
az resource wait --deleted --ids $workspaceResourceId --include-response-body true
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "cluster-subscription-id"

# Find cluster resource ID
$clusterResourceId = (Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name").id

Select-AzSubscription "workspace-subscription-id"

# Link the workspace to the cluster
Set-AzOperationalInsightsLinkedService -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -LinkedServiceName cluster -WriteAccessResourceId $clusterResourceId -AsJob

# Check when the job is done
Get-Job -Command "Set-AzOperationalInsightsLinkedService" | Format-List -Property *
```

#### [REST API](#tab/restapi)

Use the following REST call to link to a cluster:

*Send*

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2021-06-01 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```

*Response*

202 (Accepted) and header.

---

### Check workspace link status
  
When a cluster is configured with customer-managed keys, data ingested to the workspaces after the link operation completion is stored encrypted with your managed key. The workspace link operation can take up to 90 minutes to complete and you can check the state by sending Get request to workspace and observe if *clusterResourceId* property is present in the response under *features*.

#### [CLI](#tab/cli)

```azurecli
az account set --subscription "workspace-subscription-id"

az monitor log-analytics workspace show --resource-group "resource-group-name" --workspace-name "workspace-name"
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "workspace-subscription-id"

Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

#### [REST API](#tab/restapi)

*Call*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>?api-version=2021-06-01
Authorization: Bearer <token>
```

*Response*

```json
{
  "properties": {
    "source": "Azure",
    "customerId": "workspace-name",
    "provisioningState": "Succeeded",
    "sku": {
      "name": "pricing-tier-name",
      "lastSkuUpdate": "Tue, 28 Jan 2020 12:26:30 GMT"
    },
    "retentionInDays": 31,
    "features": {
      "legacy": 0,
      "searchVersion": 1,
      "enableLogAccessUsingOnlyResourcePermissions": true,
      "clusterResourceId": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name"
    },
    "workspaceCapping": {
      "dailyQuotaGb": -1.0,
      "quotaNextResetTime": "Tue, 28 Jan 2020 14:00:00 GMT",
      "dataIngestionStatus": "RespectQuota"
    }
  },
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
  "name": "workspace-name",
  "type": "Microsoft.OperationalInsights/workspaces",
  "location": "region"
}
```

---


## Change cluster properties

After you create your cluster resource and it's fully provisioned, you can edit additional properties using CLI, PowerShell or REST API. The additional properties that can be set after the cluster has been provisioned include the following:

- **keyVaultProperties** - Contains the key in Azure Key Vault with the following parameters: *KeyVaultUri*, *KeyName*, *KeyVersion*. See [Update cluster with Key identifier details](../logs/customer-managed-keys.md#update-cluster-with-key-identifier-details).
- **Identity** - The identity used to authenticate to your Key Vault. This can be System-assigned or User-assigned.
- **billingType** - Billing attribution for the cluster resource and its data. Includes on the following values:
  - **Cluster (default)**--The costs for your cluster are attributed to the cluster resource.
  - **Workspaces**--The costs for your cluster are attributed proportionately to the workspaces in the Cluster, with the cluster resource being billed some of the usage if the total ingested data for the day is under the commitment tier. See [Log Analytics Dedicated Clusters](./cost-logs.md#dedicated-clusters) to learn more about the cluster pricing model.


>[!IMPORTANT]
>Cluster update should not include both identity and key identifier details in the same operation. If you need to update both, the update should be in two consecutive operations.

> [!NOTE]
> The *billingType* property isn't supported in CLI.

## Get all clusters in resource group

#### [CLI](#tab/cli)

```azurecli
az account set --subscription "cluster-subscription-id"

az monitor log-analytics cluster list --resource-group "resource-group-name"
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "cluster-subscription-id"

Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name"
```

#### [REST API](#tab/restapi)

*Call*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2021-06-01
Authorization: Bearer <token>
```

*Response*
  
```json
{
  "value": [
    {
      "identity": {
        "type": "SystemAssigned",
        "tenantId": "tenant-id",
        "principalId": "principal-id"
      },
      "sku": {
        "name": "capacityreservation",
        "capacity": 500
      },
      "properties": {
        "provisioningState": "Succeeded",
        "clusterId": "cluster-id",
        "billingType": "Cluster",
        "lastModifiedDate": "last-modified-date",
        "createdDate": "created-date",
        "isDoubleEncryptionEnabled": false,
        "isAvailabilityZonesEnabled": false,
        "capacityReservationProperties": {
          "lastSkuUpdate": "last-sku-modified-date",
          "minCapacity": 500
        }
      },
      "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
      "name": "cluster-name",
      "type": "Microsoft.OperationalInsights/clusters",
      "location": "cluster-region"
    }
  ]
}
```

---



## Get all clusters in subscription

#### [CLI](#tab/cli)

```azurecli
az account set --subscription "cluster-subscription-id"

az monitor log-analytics cluster list
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "cluster-subscription-id"

Get-AzOperationalInsightsCluster
```
#### [REST API](#tab/restapi)

*Call*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2021-06-01
Authorization: Bearer <token>
```
    
*Response*
    
The same as for 'clusters in a resource group', but in subscription scope.

---


## Update commitment tier in cluster

When the data volume to your linked workspaces change over time and you want to update the Commitment Tier level appropriately. The tier is specified in units of GB and can have values of 500, 1000, 2000 or 5000 GB/day. Note that you don't have to provide the full REST request body but should include the sku.

#### [CLI](#tab/cli)

```azurecli
az account set --subscription "cluster-subscription-id"

az monitor log-analytics cluster update --resource-group "resource-group-name" --name "cluster-name"  --sku-capacity 500
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "cluster-subscription-id"

Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity 500
```

#### [REST API](#tab/restapi)

*Call*

```rest
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
Authorization: Bearer <token>
Content-type: application/json

{
  "sku": {
    "name": "capacityReservation",
    "Capacity": 2000
  }
}
```

---


### Update billingType in cluster

The *billingType* property determines the billing attribution for the cluster and its data:
- *Cluster* (default) -- billing is attributed to the Cluster resource
- *Workspaces* -- billing is attributed to linked workspaces proportionally. When data volume from all linked workspaces is below Commitment Tier level, the bill for the remaining volume is attributed to the cluster

#### [CLI](#tab/cli)

N/A

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "cluster-subscription-id"

Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -BillingType "Workspaces"
```

#### [REST API](#tab/restapi)

*Call*

```rest
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "billingType": "Workspaces"
    },
    "location": "region"
}
```

---

### Unlink a workspace from cluster

You can unlink a workspace from a cluster at any time. The workspace pricing tier is changed to per-GB, data ingested to cluster before the unlink operation remains in the cluster, and new data to workspace get ingested to Log Analytics. You can query data as usual and the service performs cross-cluster queries seamlessly. If cluster was configured with Customer-managed key (CMK), data remains encrypted with your key and accessible, while your key and permissions to Key Vault remain.  

> [!NOTE] 
> There is a limit of two link operations for a specific workspace within a month to prevent data distribution across clusters. Contact support if you reach limit.

Use the following commands to unlink a workspace from cluster:

#### [CLI](#tab/cli)

```azurecli
az account set --subscription "workspace-subscription-id"

az monitor log-analytics workspace linked-service delete --resource-group "resource-group-name" --workspace-name "workspace-name" --name cluster
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "workspace-subscription-id"

# Unlink a workspace from cluster
Remove-AzOperationalInsightsLinkedService -ResourceGroupName "resource-group-name" -WorkspaceName {workspace-name} -LinkedServiceName cluster
```

#### [REST API](#tab/restapi)

N/A

---


## Delete cluster

You need to have *write* permissions on the cluster resource. 

When deleting a cluster, you're losing access to all data in cluster, which was ingested from workspaces that were linked to it. This operation isn't reversible.
The cluster's billing stops when deleted, regardless the 30 days commitment tier. 

If you delete your cluster while workspaces are linked, Workspaces get automatically unlinked from the cluster before the cluster delete, and new data sent to workspaces gets ingested to Log Analytics store instead. If the retention of data in workspaces older than the period it was linked to the cluster, you can query workspace for the time range before the link to cluster and after the unlink, and the service performs cross-cluster queries seamlessly.

> [!NOTE] 
> - There is a limit of seven clusters per subscription and region, five active, plus two deleted in past 14 days.
> - Cluster's name remain reserved for 14 days after deletion, and can't be used for creating a new cluster.

Use the following commands to delete a cluster:

#### [CLI](#tab/cli)

```azurecli
az account set --subscription "cluster-subscription-id"

az monitor log-analytics cluster delete --resource-group "resource-group-name" --name $clusterName
```

#### [PowerShell](#tab/powershell)

```powershell
Select-AzSubscription "cluster-subscription-id"

Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
```

#### [REST API](#tab/restapi)

Use the following REST call to delete a cluster:

```rest
DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
Authorization: Bearer <token>
```

  **Response**

  200 OK

---



## Limits and constraints

- A maximum of five active clusters can be created in each region and subscription.

- A maximum of seven cluster allowed per subscription and region, five active, plus two deleted in past 14 days.

- A maximum of 1,000 Log Analytics workspaces can be linked to a cluster.

- A maximum of two workspace link operations on particular workspace is allowed in 30 day period.

- Moving a cluster to another resource group or subscription isn't currently supported.

- Cluster update should not include both identity and key identifier details in the same operation. In case you need to update both, the update should be in two consecutive operations.

- Lockbox isn't currently available in China. 

- [Double encryption](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) is configured automatically for clusters created from October 2020 in supported regions. You can verify if your cluster is configured for double encryption by sending a GET request on the cluster and observing that the `isDoubleEncryptionEnabled` value is `true` for clusters with Double encryption enabled. 
  - If you create a cluster and get an error "region-name doesn't support Double Encryption for clusters.", you can still create the cluster without Double encryption by adding `"properties": {"isDoubleEncryptionEnabled": false}` in the REST request body.
  - Double encryption setting can't can not be changed after the cluster has been created.

- Deleting a linked workspace is permitted while linked to cluster. If you decide to [recover](./delete-workspace.md#recover-workspace) the workspace during the [soft-delete](./delete-workspace.md#soft-delete-behavior) period, it returns to previous state and remains linked to cluster.

## Troubleshooting

- If you get conflict error when creating a cluster, it may be that you've deleted your cluster in the last 14 days and it's in a soft-delete state. The cluster name remains reserved during the soft-delete period and you can't create a new cluster with that name. The name is released after the soft-delete period when the cluster is permanently deleted.

- If you update your cluster while the cluster is at provisioning or updating state, the update will fail.

- Some operations are long and can take a while to complete. These are *cluster create*, *cluster key update* and *cluster delete*. You can check the operation status by sending GET request to cluster or workspace and observe the response. For example, unlinked workspace won't have the *clusterResourceId* under *features*.

- Workspace link to cluster will fail if it is linked to another cluster.

## Error messages
  
### Cluster Create

-  400--Cluster name is not valid. Cluster name can contain characters a-z, A-Z, 0-9 and length of 3-63.
-  400--The body of the request is null or in bad format.
-  400--SKU name is invalid. Set SKU name to capacityReservation.
-  400--Capacity was provided but SKU is not capacityReservation. Set SKU name to capacityReservation.
-  400--Missing Capacity in SKU. Set Capacity value to 500, 1000, 2000 or 5000 GB/day.
-  400--Capacity is locked for 30 days. Decreasing capacity is permitted 30 days after update.
-  400--No SKU was set. Set the SKU name to capacityReservation and Capacity value to 500, 1000, 2000 or 5000 GB/day.
-  400--Identity is null or empty. Set Identity with systemAssigned type.
-  400--KeyVaultProperties are set on creation. Update KeyVaultProperties after cluster creation.
-  400--Operation cannot be executed now. Async operation is in a state other than succeeded. Cluster must complete its operation before any update operation is performed.

### Cluster Update

-  400--Cluster is in deleting state. Async operation is in progress. Cluster must complete its operation before any update operation is performed.
-  400--KeyVaultProperties is not empty but has a bad format. See [key identifier update](../logs/customer-managed-keys.md#update-cluster-with-key-identifier-details).
-  400--Failed to validate key in Key Vault. Could be due to lack of permissions or when key doesn't exist. Verify that you [set key and access policy](../logs/customer-managed-keys.md#grant-key-vault-permissions) in Key Vault.
-  400--Key is not recoverable. Key Vault must be set to Soft-delete and Purge-protection. See [Key Vault documentation](../../key-vault/general/soft-delete-overview.md)
-  400--Operation cannot be executed now. Wait for the Async operation to complete and try again.
-  400--Cluster is in deleting state. Wait for the Async operation to complete and try again.

### Cluster Get

 -  404--Cluster not found, the cluster may have been deleted. If you try to create a cluster with that name and get conflict, the cluster is in soft-delete for 14 days. You can contact support to recover it, or use another name to create a new cluster. 

### Cluster Delete

 -  409--Can't delete a cluster while in provisioning state. Wait for the Async operation to complete and try again.

### Workspace link

-  404--Workspace not found. The workspace you specified doesn't exist or was deleted.
-  409--Workspace link or unlink operation in process.
-  400--Cluster not found, the cluster you specified doesn't exist or was deleted. If you try to create a cluster with that name and get conflict, the cluster is in soft-delete for 14 days. You can contact support to recover it.

### Workspace unlink
-  404--Workspace not found. The workspace you specified doesn't exist or was deleted.
-  409--Workspace link or unlink operation in process.

## Next steps

- Learn about [Log Analytics dedicated cluster billing](cost-logs.md#dedicated-clusters)
- Learn about [proper design of Log Analytics workspaces](../logs/workspace-design.md)

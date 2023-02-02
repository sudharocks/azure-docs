---
title: 'Discover and govern Azure Data Lake Storage (ADLS) Gen2'
description: This article outlines the process to register an Azure Data Lake Storage Gen2 data source in Microsoft Purview including instructions to authenticate and interact with the Azure Data Lake Storage Gen2 source.
author: athenads
ms.author: athenadsouza
ms.service: purview
ms.topic: how-to
ms.date: 10/04/2022
ms.custom: template-how-to, ignite-fall-2021, references_regions
---
# Connect to Azure Data Lake Storage in Microsoft Purview

This article outlines the process to register and govern an Azure Data Lake Storage (ADLS Gen2) data source in Microsoft Purview including instructions to authenticate and interact with the ADLS Gen2 source.

## Supported capabilities

|**Metadata Extraction**|  **Full Scan**  |**Incremental Scan**|**Scoped Scan**|**Classification**|**Access Policy**|**Lineage**|**Data Sharing**|
|---|---|---|---|---|---|---|---|
| [Yes](#register) | [Yes](#scan)|[Yes](#scan) | [Yes](#scan)|[Yes](#scan)| [Yes (preview)](#access-policy)  | Limited* |[Yes](#data-sharing)|

\* *Lineage is supported if dataset is used as a source/sink in [Data Factory](how-to-link-azure-data-factory.md) or [Synapse pipeline](how-to-lineage-azure-synapse-analytics.md).*

When scanning Azure Data Lake Storage Gen2 source, Microsoft Purview supports extracting technical metadata including:

- Storage account
- Data Lake Storage Gen2 Service
- File system (container)
- Folders
- Files
- Resource sets

When setting up scan, you can choose to scan the entire ADLS Gen2 or selective folders. Learn about the supported file format [here](microsoft-purview-connector-overview.md#file-types-supported-for-scanning).

## Prerequisites

* An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* An active [Microsoft Purview account](create-catalog-portal.md).

* You'll need to be a Data Source Administrator and Data Reader to register a source and manage it in the Microsoft Purview governance portal. See our [Microsoft Purview Permissions page](catalog-permissions.md) for details.

* You need to have at least [Reader permission on the ADLS Gen 2 account](../storage/blobs/data-lake-storage-access-control-model.md#role-based-access-control-azure-rbac) to be able to register it.

## Register
This section will enable you to register the ADLS Gen2 data source for scan and data share in Purview.

### Prerequisites for register
* You'll need to be a Data Source Admin and one of the other Purview roles (for example, Data Reader or Data Share Contributor) to register a source and manage it in the Microsoft Purview governance portal. See our [Microsoft Purview Permissions page](catalog-permissions.md) for details.

### Steps to register

It's important to register the data source in Microsoft Purview prior to setting up a scan for the data source.

1. Go to the [Azure portal](https://portal.azure.com), and navigate to the **Microsoft Purview accounts** page and select your _Purview account_

1. **Open Microsoft Purview governance portal** and navigate to the **Data Map --> Sources**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-open-purview-studio.png" alt-text="Screenshot that shows the link to open Microsoft Purview governance portal":::

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-sources.png" alt-text="Screenshot that navigates to the Sources link in the Data Map":::

1. Create the [Collection hierarchy](./quickstart-create-collection.md) using the **Collections** menu and assign permissions to individual subcollections, as required

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-collections.png" alt-text="Screenshot that shows the collection menu to create collection hierarchy":::

1. Navigate to the appropriate collection under the **Sources** menu and select the **Register** icon to register a new ADLS Gen2 data source

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-register-source.png" alt-text="Screenshot that shows the collection used to register the data source":::

1. Select the **Azure Data Lake Storage Gen2** data source and select **Continue**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-select-data-source.png" alt-text="Screenshot that allows selection of the data source":::

1. Provide a suitable **Name** for the data source, select the relevant **Azure subscription**, existing **Data Lake Store account name** and the **collection** and select **Apply**. Leave the **Data Use Management** toggle on the **disabled** position until you have a chance to carefully go over this [document](./how-to-policies-data-owner-storage.md).

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-data-source-details.png" alt-text="Screenshot that shows the details to be entered in order to register the data source":::

1. The ADLS Gen2 storage account will be shown under the selected Collection

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-data-source-collection.png" alt-text="Screenshot that shows the data source mapped to the collection to initiate scanning":::

## Scan

> [!TIP]
> To troubleshoot any issues with scanning:
> 1. Confirm you have properly set up [**authentication for scanning**](#authentication-for-a-scan)
> 1. Review our [**scan troubleshooting documentation**](troubleshoot-connections.md).

### Authentication for a scan

Your Azure network may allow for communications between your Azure resources, but if you've set up firewalls, private endpoints, or virtual networks within Azure, you'll need to follow one of these configurations below.

|Networking constraints  |Integration runtime type  |Available credential types  |
|---------|---------|---------|
|No private endpoints or firewalls | Azure IR | Managed identity (Recommended), service principal, or account key|
|Firewall enabled but no private endpoints| Azure IR | Managed identity |
|Private endpoints enabled | *Self-Hosted IR | Service principal, account key|

*To use a self-hosted integration runtime, you'll first need to [create one](manage-integration-runtimes.md) and confirm your [network settings for Microsoft Purview](catalog-private-link.md)

# [System or user assigned managed identity](#tab/MI)

#### Using a system or user assigned managed identity for scanning

There are two types of managed identity you can use:

* **System-assigned managed identity (Recommended)** - As soon as the Microsoft Purview Account is created, a system-assigned managed identity (SAMI) is created automatically in Azure AD tenant. Depending on the type of resource, specific RBAC role assignments are required for the Microsoft Purview system-assigned managed identity (SAMI) to perform the scans.

* **User-assigned managed identity** (preview) - Similar to a system managed identity, a user-assigned managed identity (UAMI) is a credential resource that can be used to allow Microsoft Purview to authenticate against Azure Active Directory. For more information, you can see our [User-assigned managed identity guide](manage-credentials.md#create-a-user-assigned-managed-identity).

It's important to give your Microsoft Purview account or user-assigned managed identity (UAMI) the permission to scan the ADLS Gen2 data source. You can add your Microsoft Purview account's system-assigned managed identity (which has the same name as your Microsoft Purview account) or UAMI at the Subscription, Resource Group, or Resource level, depending on what level scan permissions are needed.

> [!Note]
> You need to be an owner of the subscription to be able to add a managed identity on an Azure resource.

1. From the [Azure portal](https://portal.azure.com), find either the subscription, resource group, or resource (for example, an Azure Data Lake Storage Gen2 storage account) that you would like to allow the catalog to scan.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-storage-acct.png" alt-text="Screenshot that shows the storage account":::

1. Select **Access Control (IAM)** in the left navigation and then select **+ Add** --> **Add role assignment**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-access-control.png" alt-text="Screenshot that shows the access control for the storage account":::

1. Set the **Role** to **Storage Blob Data Reader** and enter your _Microsoft Purview account name_ or _[user-assigned managed identity](manage-credentials.md#create-a-user-assigned-managed-identity)_ under the **Select** input box. Then, select **Save** to give this role assignment to your Microsoft Purview account.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-assign-permissions.png" alt-text="Screenshot that shows the details to assign permissions for the Microsoft Purview account":::

    > [!Note]
    > For more details, please see steps in [Authorize access to blobs and queues using Azure Active Directory](../storage/blobs/authorize-access-azure-active-directory.md)

    > [!NOTE]
    > If you have firewall enabled for the storage account, you must use **managed identity** authentication method when setting up a scan.

1. Go into your ADLS Gen2 storage account in [Azure portal](https://portal.azure.com)
1. Navigate to **Security + networking > Networking**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-networking.png" alt-text="Screenshot that shows the details to provide firewall access":::

1. Choose **Selected Networks** under **Allow access from**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-network-access.png" alt-text="Screenshot that shows the details to allow access to selected networks":::

1. In the **Exceptions** section, select **Allow trusted Microsoft services to access this storage account** and hit **Save**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-permission-microsoft-services.png" alt-text="Screenshot that shows the exceptions to allow trusted Microsoft services to access the storage account":::

# [Account Key](#tab/AK)

#### Using Account Key for scanning

> [!Note]
> If you use this option, you need to deploy an _Azure key vault_ resource in your subscription and [assign _Microsoft Purview account’s_ System Assigned Managed Identity (SAMI) required access permission to secrets inside _Azure key vault_.](manage-credentials.md#microsoft-purview-permissions-on-the-azure-key-vault)

When authentication method selected is **Account Key**, you need to get your access key and store in the key vault:

1. Navigate to your ADLS Gen2 storage account
1. Select **Security + networking > Access keys**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-access-keys.png" alt-text="Screenshot that shows the access keys in the storage account":::

1. Copy your *key* and save it separately for the next steps

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-key.png" alt-text="Screenshot that shows the access keys to be copied":::

1. Navigate to your key vault

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-key-vault.png" alt-text="Screenshot that shows the key vault":::

1. Select **Settings > Secrets** and select **+ Generate/Import**
:::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-generate-secret.png" alt-text="Screenshot that shows the key vault option to generate a secret":::

1. Enter the **Name** and **Value** as the *key* from your storage account

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-secret-values.png" alt-text="Screenshot that shows the key vault option to enter the secret values":::

1. Select **Create** to complete

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-secret.png" alt-text="Screenshot that shows the key vault option to create a secret":::

1. If your key vault isn't connected to Microsoft Purview yet, you'll need to [create a new key vault connection](manage-credentials.md#create-azure-key-vaults-connections-in-your-microsoft-purview-account)
1. Finally, [create a new credential](manage-credentials.md#create-a-new-credential) using the key to set up your scan

# [Service Principal](#tab/SP)

#### Using Service Principal for scanning

##### Creating a new service principal

If you need to [Create a new service principal](./create-service-principal-azure.md), it's required to register an application in your Azure AD tenant and provide access to Service Principal in your data sources. Your Azure AD Global Administrator or other roles such as Application Administrator can perform this operation.

##### Getting the Service Principal's Application ID

1. Copy the **Application (client) ID** present in the **Overview** of the [_Service Principal_](./create-service-principal-azure.md) already created

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-sp-appln-id.png" alt-text="Screenshot that shows the Application (client) ID for the Service Principal":::

##### Granting the Service Principal access to your ADLS Gen2 account

It's important to give your service principal the permission to scan the ADLS Gen2 data source. You can add access for the service principal at the Subscription, Resource Group, or Resource level, depending on what level scan permissions are needed.

> [!Note]
> You need to be an owner of the subscription to be able to add a service principal on an Azure resource.

1. From the [Azure portal](https://portal.azure.com), find either the subscription, resource group, or resource (for example, an Azure Data Lake Storage Gen2 storage account) that you would like to allow the catalog to scan.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-storage-acct.png" alt-text="Screenshot that shows the storage account":::

1. Select **Access Control (IAM)** in the left navigation and then select **+ Add** --> **Add role assignment**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-access-control.png" alt-text="Screenshot that shows the access control for the storage account":::

1. Set the **Role** to **Storage Blob Data Reader** and enter your _service principal_ under **Select** input box. Then, select **Save** to give this role assignment to your Microsoft Purview account.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-sp-permission.png" alt-text="Screenshot that shows the details to provide storage account permissions to the service principal":::

---

### Create the scan

1. Open your **Microsoft Purview account** and select the **Open Microsoft Purview governance portal**
1. Navigate to the **Data map** --> **Sources** to view the collection hierarchy
1. Select the **New Scan** icon under the **ADLS Gen2 data source** registered earlier

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-new-scan.png" alt-text="Screenshot that shows the screen to create a new scan":::

# [System or user assigned managed identity](#tab/MI)

#### If using a system or user assigned managed identity

1. Provide a **Name** for the scan, select the system-assigned or user-assigned managed identity under **Credential**, choose the appropriate collection for the scan, and select **Test connection**. On a successful connection, select **Continue**.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-managed-identity.png" alt-text="Screenshot that shows the managed identity option to run the scan":::

# [Account Key](#tab/AK)

#### If using Account Key

1. Provide a **Name** for the scan, select the Azure IR or your Self-Hosted IR depending on your configuration, choose the appropriate collection for the scan, and select **+ New** under credential.

1. Select **Account Key** as the authentication method, then select the appropriate **Key vault connection**, and provide the name of the secret you used to store the account key. Then select **Create**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-acct-key.png" alt-text="Screenshot that shows the Account Key option for scanning":::

1. Select **Test connection**. On a successful connection, select **Continue**

# [Service Principal](#tab/SP)

#### If using Service Principal

1. Provide a **Name** for the scan, select the Azure IR or your Self-Hosted IR depending on your configuration, choose the appropriate collection for the scan, and select the **+ New** under **Credential**

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-sp-option.png" alt-text="Screenshot that shows the option for service principal to enable scanning":::

1. Select the appropriate **Key vault connection** and the **Secret name** that was used while creating the _Service Principal_. The **Service Principal ID** is the **Application (client) ID** copied earlier.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-service-principal-option.png" alt-text="Screenshot that shows the service principal option":::

1. Select **Test connection**. On a successful connection, select **Continue**

---

### Scope and run the scan

1. You can scope your scan to specific folders and subfolders by choosing the appropriate items in the list.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-scope-scan.png" alt-text="Scope your scan":::

1. Then select a scan rule set. You can choose between the system default, existing custom rule sets, or create a new rule set inline.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-scan-rule-set.png" alt-text="Scan rule set":::

1. If creating a new _scan rule set_, select the **file types** to be included in the scan rule.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-file-types.png" alt-text="Scan rule set file types":::

1. You can select the **classification rules** to be included in the scan rule

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-classification rules.png" alt-text="Scan rule set classification rules":::

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-select-scan-rule-set.png" alt-text="Scan rule set selection":::

1. Choose your scan trigger. You can set up a schedule or run the scan once.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-scan-trigger.png" alt-text="scan trigger":::

1. Review your scan and select **Save and run**.

    :::image type="content" source="media/register-scan-adls-gen2/register-adls-gen2-review-scan.png" alt-text="review scan":::

[!INCLUDE [view and manage scans](includes/view-and-manage-scans.md)]

## Data sharing
Microsoft Purview Data Sharing (preview) enables sharing of data in-place from ADLS Gen2 to ADLS Gen2. This section provides details about the ADLS Gen2 specific requirements for sharing and receiving data in-place. Refer to [How to share data](how-to-share-data.md) and [How to receive share](how-to-receive-share.md) for step by step guide on how to use data share.

### Storage accounts supported for in-place data sharing
The following storage accounts are supported for in-place data sharing:

* Regions: Canada Central, Canada East, UK South, UK West, Australia East, Japan East, Korea South, and South Africa North
* Redundancy options: LRS, GRS, RA-GRS
* Tiers: Hot, Cool

Only use storage account without production workload for the preview.

>[!NOTE]
> Source and target storage accounts must be in the same region as each other. They don't need to be in the same region as the Microsoft Purview account.

### Storage account permissions required to share data
To add or update a storage account asset to a share, you need ONE of the following permissions:

* **Microsoft.Authorization/roleAssignments/write** - This permission is available in the *Owner* role.
* **Microsoft.Storage/storageAccounts/blobServices/containers/blobs/modifyPermissions/** - This permission is available in the *Blob Storage Data Owner* role.

### Storage account permissions required to receive shared data
To map a storage account asset in a received share, you need ONE of the following permissions:

* **Microsoft.Storage/storageAccounts/write** - This permission is  available in the *Contributor* and *Owner* role.
* **Microsoft.Storage/storageAccounts/blobServices/containers/write** - This permission is available in the *Contributor*, *Owner*, *Storage Blob Data Contributor* and *Storage Blob Data Owner* role.

### Update shared data in source storage account
Updates you make to shared files or data in the shared folder from source storage account will be made available to recipient in target storage account in near real time. When you delete subfolder or files within the shared folder, they'll disappear for recipient. To delete the shared folder, file or parent folders or containers, you need to first revoke access to all your shares from the source storage account.

### Access shared data in target storage account
The target storage account enables recipient to access the shared data read-only in near real time. You can connect analytics tools such as Synapse Workspace and Databricks to the shared data to perform analytics. Cost of accessing the shared data is charged to the target storage account. 

### Service limit
Source storage account can support up to 20 targets, and target storage account can support up to 100 sources. If you require an increase in limit, contact Support.

## Access policy

### Supported policies
The following types of policies are supported on this data resource from Microsoft Purview:
- [Data owner policies](concept-policies-data-owner.md)
- [Self-service access policies](concept-self-service-data-access-policy.md)

### Access policy pre-requisites on Azure Storage accounts
[!INCLUDE [Access policies Azure Storage specific pre-requisites](./includes/access-policies-prerequisites-storage.md)]

### Configure the Microsoft Purview account for policies
[!INCLUDE [Access policies generic configuration](./includes/access-policies-configuration-generic.md)]

### Register the data source in Microsoft Purview for Data Use Management
The Azure Storage resource needs to be registered first with Microsoft Purview before you can create access policies.
To register your resource, follow the **Prerequisites** and **Register** sections of this guide:
-   [Register Azure Data Lake Storage (ADLS) Gen2 in Microsoft Purview](register-scan-adls-gen2.md#prerequisites)

After you've registered the data source, you'll need to enable Data Use Management. This is a pre-requisite before you can create policies on the data source. Data Use Management can impact the security of your data, as it delegates to certain Microsoft Purview roles managing access to the data sources. **Go through the secure practices related to Data Use Management in this guide**: [How to enable Data Use Management](./how-to-enable-data-use-management.md) 

Once your data source has the  **Data Use Management** option set to **Enabled**, it will look like this screenshot:
![Screenshot shows how to register a data source for policy with the option Data use management set to enable](./media/how-to-policies-data-owner-storage/register-data-source-for-policy-storage.png)

### Create a policy
To create an access policy for Azure Data Lake Storage Gen2, follow this guide:
* [Data owner policy on a single storage account](./how-to-policies-data-owner-storage.md#create-and-publish-a-data-owner-policy)

To create policies that cover all data sources inside a resource group or Azure subscription you can refer to [this section](register-scan-azure-multiple-sources.md#access-policy).

## Next steps
Follow the below guides to learn more about Microsoft Purview and your data.
- [Data owner policies in Microsoft Purview](concept-policies-data-owner.md)
- [Data Estate Insights in Microsoft Purview](concept-insights.md)
- [Lineage in Microsoft Purview](catalog-lineage-user-guide.md)
- [Data share in Microsoft Purview](concept-data-share.md)

---
title: 'Restore a soft deleted enterprise application'
description: Restore a soft deleted enterprise application in Azure Active Directory.
services: active-directory
author: omondiatieno
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 11/02/2022
ms.author: jomondi
ms.reviewer: sureshja
ms.custom: mode-other
zone_pivot_groups: enterprise-apps-minus-portal
#Customer intent: As an administrator of an Azure AD tenant, I want to restore a soft deleted enterprise application.
---

# Restore an enterprise application in Azure AD

In this article, you'll learn how to restore a soft deleted enterprise application in your Azure Active Directory (Azure AD) tenant. Soft deleted enterprise applications can be restored from the recycle bin within the first 30 days after their deletion. After the 30-day window, the enterprise application is permanently deleted and can't be restored.

>[!IMPORTANT]
>If you deleted an [application registration](../develop/howto-remove-app.md) in its home tenant through app registrations in the Azure portal, the enterprise application, which is its corresponding service principal also got deleted. If you restore the deleted application registration through the Azure portal, its corresponding service principal, won't be restored. Instead, this action will create a new service principal. Therefore, if you had configurations on the previous enterprise application, you can't restore them through the Azure portal. Use the workaround provided in this article to recover the deleted service principal and its previous configurations.
## Prerequisites

To restore an enterprise application, you need:

- An Azure AD user account. If you don't already have one, you can [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- One of the following roles: Global Administrator, Cloud Application Administrator, Application Administrator, or owner of the service principal.
- A [soft deleted enterprise application](delete-application-portal.md) in your tenant.
## View restorable enterprise applications

To recover your enterprise application with its previous configurations, first delete the enterprise application that was restored through the Azure portal, then take the following steps to recover the soft deleted enterprise application. For more information on frequently asked questions about deletion and recovery of applications, see [Deleting and recovering applications FAQs](delete-recover-faq.yml).

:::zone pivot="aad-powershell"

> [!IMPORTANT]
> Make sure you're using the AzureAD module. This is important if you've installed both the [AzureAD](/powershell/module/azuread/?preserve-view=true&view=azureadps-2.0) module and the AzureADPreview module.
1. Run the following commands:

    ```powershell
    Remove-Module AzureADPreview
    Import-Module AzureAD
    ```

1. Connect to Azure AD PowerShell:

   ```powershell
   Connect-AzureAD
   ```

1. To view the recently deleted enterprise application, run the following command:

   ```powershell
   Get-AzureADMSDeletedDirectoryObject -Id <id>
   ```

Replace id with the object ID of the service principal that you want to restore.
 
:::zone-end

:::zone pivot="ms-powershell"

1. Run `connect-MgGraph -Scopes "Application.ReadWrite.All"` and sign in with a Global Administrator user account.

1. To view the recently deleted enterprise applications, run the following command:
   
   ```powershell
   Get-MgDirectoryDeletedItem -DirectoryObjectId <id>
   ```
Replace id with the object ID of the service principal that you want to restore.

:::zone-end

:::zone pivot="ms-graph"

View and restore recently deleted enterprise applications using [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer).

To get the list of deleted enterprise applications in your tenant, run the following query.
   
   ```http
   GET https://graph.microsoft.com/v1.0/directory/deletedItems/microsoft.graph.servicePrincipal
   ```
From the list of deleted service principals generated, record the ID of the enterprise application you want to restore.

Alternatively, if you want to get the specific enterprise application that was deleted, fetch the deleted service principal and filter the results by the client's application ID (appId) property using the following syntax:

`https://graph.microsoft.com/v1.0/directory/deletedItems/microsoft.graph.servicePrincipal?$filter=appId eq '{appId}'`. Once you've retrieved the object ID of the deleted service principal, proceed to restore it.

:::zone-end

## Restore an enterprise application  

:::zone pivot="aad-powershell"

1. To restore the enterprise application, run the following command:


   ```powershell  
   Restore-AzureADMSDeletedDirectoryObject -Id <id>
   ```

Replace id with the object ID of the service principal that you want to restore.

:::zone-end

:::zone pivot="ms-powershell"

1. To restore the enterprise application, run the following command:

   ```powershell   
   Restore-MgDirectoryObject -DirectoryObjectId <id>
   ```

Replace id with the object ID of the service principal that you want to restore.

:::zone-end
   
:::zone pivot="ms-graph"

1. To restore the enterprise application, run the following query:
   
   ```http
   POST https://graph.microsoft.com/v1.0/directory/deletedItems/{id}/restore
   ```

Replace id with the object ID of the service principal that you want to restore.

:::zone-end

## Permanently delete an enterprise application

>[!WARNING]
> Permanently deleting an enterprise application is an irreversible action. Any present configurations on the app will be completely lost. Carefully review the details of the enterprise application to be sure you still want to hard delete it.

:::zone pivot="aad-powershell"

To permanently delete a soft deleted enterprise application, run the following command:

```powershell
Remove-AzureADMSDeletedDirectoryObject -Id <id>
```
:::zone-end

:::zone pivot="ms-powershell"
   
1. To permanently delete the soft deleted enterprise application, run the following command:
   
   ```powershell
   Remove-MgDirectoryDeletedItem -DirectoryObjectId <id>
   ``` 

:::zone-end

:::zone pivot="ms-graph"

To permanently delete a soft deleted enterprise application, run the following query in Microsoft Graph explorer

```http
DELETE https://graph.microsoft.com/v1.0/directory/deletedItems/{object-id}
```

:::zone-end

## Next steps

- [Recovery and deletion FAQ](delete-recover-faq.yml)
- [Applications and service principals](../develop/app-objects-and-service-principals.md)

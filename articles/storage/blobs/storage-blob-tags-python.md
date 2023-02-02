---
title: Use blob index tags to manage and find data with Python
titleSuffix: Azure Storage
description: Learn how to categorize, manage, and query for blob objects by using the Python client library.  
services: storage
author: pauljewellmsft

ms.author: pauljewell
ms.date: 01/25/2023
ms.service: storage
ms.subservice: blobs
ms.topic: how-to
ms.devlang: python
ms.custom: devx-track-python, devguide-python
---

# Use blob index tags to manage and find data with Python

This article shows how to use blob index tags to manage and find data using the [Azure Storage client library for Python](/python/api/overview/azure/storage). 

Blob index tags categorize data in your storage account using key-value tag attributes. These tags are automatically indexed and exposed as a searchable multi-dimensional index to easily find data. This article shows you how to set, get, and find data using blob index tags.

To learn more about this feature along with known issues and limitations, see [Manage and find Azure Blob data with blob index tags](storage-manage-find-blobs.md).

## Set tags

You can set index tags if your code has authorized access to blob data through one of the following mechanisms:
- Security principal that is assigned an Azure RBAC role with the [Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/write](../../role-based-access-control/resource-provider-operations.md#microsoftstorage) action. The [Storage Blob Data Owner](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) is a built-in role that includes this action.
- Shared Access Signature with permission to access the blob's tags (`t` permission)
- Account key

For more information, see [Setting blob index tags](storage-manage-find-blobs.md#setting-blob-index-tags).

You can set tags by using the following method:

- [BlobClient.set_blob_tags](/python/api/azure-storage-blob/azure.storage.blob.blobclient#azure-storage-blob-blobclient-set-blob-tags)

The specified tags in this method will replace existing tags. If old values must be preserved, they must be downloaded and included in the call to this method. The following example shows how to set tags:

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/blob-devguide-py/blob-devguide-blobs.py" id="Snippet_set_blob_tags":::

You can delete all tags by passing an empty `dict` object into the `set_blob_tags` method:

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/blob-devguide-py/blob-devguide-blobs.py" id="Snippet_clear_blob_tags":::

## Get tags

You can get index tags if your code has authorized access to blob data through one of the following mechanisms:
- Security principal that is assigned an Azure RBAC role with the [Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/read](../../role-based-access-control/resource-provider-operations.md#microsoftstorage) action. The [Storage Blob Data Owner](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) is a built-in role that includes this action.
- Shared Access Signature with permission to access the blob's tags (`t` permission)
- Account key

For more information, see [Getting and listing blob index tags](storage-manage-find-blobs.md#getting-and-listing-blob-index-tags).

You can get tags by using the following method: 

- [BlobClient.get_blob_tags](/python/api/azure-storage-blob/azure.storage.blob.blobclient#azure-storage-blob-blobclient-get-blob-tags)

The following example shows how to retrieve and iterate over the blob's tags:

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/blob-devguide-py/blob-devguide-blobs.py" id="Snippet_get_blob_tags":::

## Filter and find data with blob index tags

You can use index tags to find and filter data if your code has authorized access to blob data through one of the following mechanisms:
- Security principal that is assigned an Azure RBAC role with the [Microsoft.Storage/storageAccounts/blobServices/containers/blobs/filter/action](../../role-based-access-control/resource-provider-operations.md#microsoftstorage) action. The [Storage Blob Data Owner](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) is a built-in role that includes this action.
- Shared Access Signature with permission to find blobs by tags (`f` permission)
- Account key

For more information, see [Finding data using blob index tags](storage-manage-find-blobs.md#finding-data-using-blob-index-tags).

> [!NOTE]
> You can't use index tags to retrieve previous versions. Tags for previous versions aren't passed to the blob index engine. For more information, see [Conditions and known issues](storage-manage-find-blobs.md#conditions-and-known-issues).

You can find data by using the following method: 

- [ContainerClient.find_blobs_by_tag](/python/api/azure-storage-blob/azure.storage.blob.containerclient#azure-storage-blob-containerclient-find-blobs-by-tags)

The following example finds and lists all blobs tagged as an image:

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/blob-devguide-py/blob-devguide-blobs.py" id="Snippet_find_blobs_by_tags":::

## Resources

To learn more about how to use index tags to manage and find data using the Azure Blob Storage client library for Python, see the following resources.

### REST API operations

The Azure SDK for Python contains libraries that build on top of the Azure REST API, allowing you to interact with REST API operations through familiar Python paradigms. The client library methods for managing and using blob index tags use the following REST API operations:

- [Get Blob Tags](/rest/api/storageservices/get-blob-tags) (REST API)
- [Set Blob Tags](/rest/api/storageservices/set-blob-tags) (REST API)
- [Find Blobs by Tags](/rest/api/storageservices/find-blobs-by-tags) (REST API)

### Code samples

- [View code samples from this article (GitHub)](https://github.com/Azure-Samples/AzureStorageSnippets/blob/master/blobs/howto/python/blob-devguide-py/blob-devguide-blobs.py)

[!INCLUDE [storage-dev-guide-resources-python](../../../includes/storage-dev-guides/storage-dev-guide-resources-python.md)]

### See also

- [Manage and find Azure Blob data with blob index tags](storage-manage-find-blobs.md)
- [Use blob index tags to manage and find data on Azure Blob Storage](storage-blob-index-how-to.md)
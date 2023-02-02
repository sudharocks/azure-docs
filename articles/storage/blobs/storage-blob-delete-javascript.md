---
title: Delete and restore a blob with JavaScript
titleSuffix: Azure Storage
description: Learn how to delete and restore a blob in your Azure Storage account using the JavaScript client library
services: storage
author: pauljewellmsft
ms.author: pauljewell
ms.date: 11/30/2022
ms.service: storage
ms.subservice: blobs
ms.topic: how-to
ms.devlang: javascript
ms.custom: devx-track-js, devguide-js
---

# Delete and restore a blob with JavaScript

This article shows how to delete blobs with the [Azure Storage client library for JavaScript](https://www.npmjs.com/package/@azure/storage-blob). If you've enabled [soft delete for blobs](soft-delete-blob-overview.md), you can restore deleted blobs during the retention period.

The [sample code snippets](https://github.com/Azure-Samples/AzureStorageSnippets/tree/master/blobs/howto/JavaScript/NodeJS-v12/dev-guide) are available in GitHub as runnable Node.js files.

> [!NOTE]
> The examples in this article assume that you've created a [BlobServiceClient](/javascript/api/@azure/storage-blob/blobserviceclient) object by using the guidance in the [Get started with Azure Blob Storage and JavaScript](storage-blob-javascript-get-started.md) article. Blobs in Azure Storage are organized into containers. Before you can upload a blob, you must first create a container. To learn how to create a container, see [Create a container in Azure Storage with JavaScript](storage-blob-container-create.md).

## Delete a blob

To delete a blob, create a [BlobClient](storage-blob-javascript-get-started.md#create-a-blobclient-object) then call either of these methods:

- [BlobClient.delete](/javascript/api/@azure/storage-blob/blobclient#@azure-storage-blob-blobclient-delete)
- [BlobClient.deleteIfExists](/javascript/api/@azure/storage-blob/blobclient#@azure-storage-blob-blobclient-deleteifexists)

The following example deletes a blob.

```javascript
async function deleteBlob(containerClient, blobName){

  // include: Delete the base blob and all of its snapshots.
  // only: Delete only the blob's snapshots and not the blob itself.
  const options = {
    deleteSnapshots: 'include' // or 'only'
  }

  // Create blob client from container client
  const blockBlobClient = await containerClient.getBlockBlobClient(blobName);

  await blockBlobClient.delete(options);

  console.log(`deleted blob ${blobName}`);

}
```

The following example deletes a blob if it exists.

```javascript
async function deleteBlobIfItExists(containerClient, blobName){

  // include: Delete the base blob and all of its snapshots.
  // only: Delete only the blob's snapshots and not the blob itself.
  const options = {
    deleteSnapshots: 'include' // or 'only'
  }

  // Create blob client from container client
  const blockBlobClient = await containerClient.getBlockBlobClient(blobName);

  await blockBlobClient.deleteIfExists(options);

  console.log(`deleted blob ${blobName}`);

}
```

## Restore a deleted blob

Blob soft delete protects an individual blob and its versions, snapshots, and metadata from accidental deletes or overwrites by maintaining the deleted data in the system for a specified period of time. During the retention period, you can restore the blob to its state at deletion. After the retention period has expired, the blob is permanently deleted. For more information about blob soft delete, see [Soft delete for blobs](soft-delete-blob-overview.md).

You can use the Azure Storage client libraries to restore a soft-deleted blob or snapshot. 

#### Restore soft-deleted objects when versioning is disabled

To restore deleted blobs, create a [BlobClient](storage-blob-javascript-get-started.md#create-a-blobclient-object) then call the following method:

- [BlobClient.undelete](/javascript/api/@azure/storage-blob/blobclient#@azure-storage-blob-blobclient-undelete)

This method restores soft-deleted blobs and any deleted snapshots associated with it. Calling this method for a blob that has not been deleted has no effect. 

```javascript
async function undeleteBlob(containerClient, blobName){

  // Create blob client from container client
  const blockBlobClient = await containerClient.getBlockBlobClient(blobName);

  await blockBlobClient.undelete();

  console.log(`undeleted blob ${blobName}`);

}
```

## See also

- [Get started with Azure Blob Storage and JavaScript](storage-blob-javascript-get-started.md)
- [Delete Blob](/rest/api/storageservices/delete-blob) (REST API)
- [Soft delete for blobs](soft-delete-blob-overview.md)
- [Undelete Blob](/rest/api/storageservices/undelete-blob) (REST API)

---
title: Delete and restore a blob container with Java
titleSuffix: Azure Storage
description: Learn how to delete and restore a blob container in your Azure Storage account using the Java client library.
services: storage
author: pauljewellmsft

ms.service: storage
ms.topic: how-to
ms.date: 11/15/2022
ms.author: pauljewell
ms.subservice: blobs
ms.devlang: java
ms.custom: devx-track-java, devguide-java
---

# Delete and restore a blob container with Java

This article shows how to delete containers with the [Azure Storage client library for Java](/java/api/overview/azure/storage-blob-readme). If you've enabled [container soft delete](soft-delete-container-overview.md), you can restore deleted containers.

## Delete a container

To delete a container in Java, use one of the following methods from the `BlobServiceClient` class:

- [deleteBlobContainer](/java/api/com.azure.storage.blob.blobserviceclient)
- [deleteBlobContainerIfExists](/java/api/com.azure.storage.blob.blobserviceclient)

You can also delete a container using one of the following methods from the `BlobContainerClient` class:

- [delete](/java/api/com.azure.storage.blob.blobcontainerclient)
- [deleteIfExists](/java/api/com.azure.storage.blob.blobcontainerclient)

After you delete a container, you can't create a container with the same name for *at least* 30 seconds. Attempting to create a container with the same name will fail with HTTP error code `409 (Conflict)`. Any other operations on the container or the blobs it contains will fail with HTTP error code `404 (Not Found)`.

The following example uses a `BlobServiceClient` object to delete the specified container:

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/blob-devguide/blob-devguide-containers/src/main/java/com/blobs/devguide/containers/ContainerDelete.java" id="Snippet_DeleteContainer":::

The following example shows how to delete all containers that start with a specified prefix:

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/blob-devguide/blob-devguide-containers/src/main/java/com/blobs/devguide/containers/ContainerDelete.java" id="Snippet_DeleteContainersPrefix":::

## Restore a deleted container

When container soft delete is enabled for a storage account, a deleted container and its contents may be recovered within a specified retention period. To learn more about container soft delete, see [Enable and manage soft delete for containers](soft-delete-container-enable.md). You can restore a soft deleted container by calling the following method of the `BlobServiceClient` class:

- [undeleteBlobContainer](/java/api/com.azure.storage.blob.blobserviceclient)

The following example finds a deleted container, gets the version of that deleted container, and then passes the version into the `undeleteBlobContainer` method to restore the container.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/blob-devguide/blob-devguide-containers/src/main/java/com/blobs/devguide/containers/ContainerDelete.java" id="Snippet_RestoreContainer":::

## See also

- [View code sample in GitHub](https://github.com/Azure-Samples/AzureStorageSnippets/blob/master/blobs/howto/Java/blob-devguide/blob-devguide-containers/src/main/java/com/blobs/devguide/containers/ContainerDelete.java)
- [Quickstart: Azure Blob Storage client library for Java](storage-quickstart-blobs-java.md)
- [Delete Container](/rest/api/storageservices/delete-container) (REST API)
- [Soft delete for containers](soft-delete-container-overview.md)
- [Enable and manage soft delete for containers](soft-delete-container-enable.md)

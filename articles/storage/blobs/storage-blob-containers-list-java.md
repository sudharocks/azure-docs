---
title: List blob containers with Java
titleSuffix: Azure Storage
description: Learn how to list blob containers in your Azure Storage account using the Java client library.
services: storage
author: pauljewellmsft

ms.service: storage
ms.topic: how-to
ms.date: 11/16/2022
ms.author: pauljewell
ms.subservice: blobs
ms.devlang: java
ms.custom: devx-track-java, devguide-java
---

# List blob containers with Java

When you list the containers in an Azure Storage account from your code, you can specify several options to manage how results are returned from Azure Storage. This article shows how to list containers using the [Azure Storage client library for Java](/java/api/overview/azure/storage-blob-readme).

## Understand container listing options

To list containers in your storage account, call the following method:

- [listBlobContainers](/java/api/com.azure.storage.blob.blobserviceclient)

The overloads for this method provide additional options for managing how containers are returned by the listing operation. These options are described in the following sections.

### Manage how many results are returned

By default, a listing operation returns up to 5000 results at a time. To return a smaller set of results, provide a nonzero value for the size of the page of results to return.

### Filter results with a prefix

To filter the list of containers, specify a string for the `prefix` parameter. The prefix string can include one or more characters. Azure Storage then returns only the containers whose names start with that prefix.

## Example: List containers

The following example list containers and filters the results by a specified prefix:

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/blob-devguide/blob-devguide-containers/src/main/java/com/blobs/devguide/containers/ContainerList.java" id="Snippet_ListContainers":::

You can also return a smaller set of results, by specifying the size of the page of results to return:

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/blob-devguide/blob-devguide-containers/src/main/java/com/blobs/devguide/containers/ContainerList.java" id="Snippet_ListContainersWithPaging":::

## See also

- [View code sample in GitHub](https://github.com/Azure-Samples/AzureStorageSnippets/blob/master/blobs/howto/Java/blob-devguide/blob-devguide-containers/src/main/java/com/blobs/devguide/containers/ContainerList.java)
- [Quickstart: Azure Blob Storage client library for Java](storage-quickstart-blobs-java.md)
- [List Containers](/rest/api/storageservices/list-containers2) (REST API)
- [Enumerating Blob Resources](/rest/api/storageservices/enumerating-blob-resources)
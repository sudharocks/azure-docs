---
title: List blobs with .NET
titleSuffix: Azure Storage
description: Learn how to list blobs in your storage account using the Azure Storage client library for .NET. Code examples show how to list blobs in a flat listing, or how to list blobs hierarchically, as though they were organized into directories or folders.
services: storage
author: pauljewellmsft
ms.author: pauljewell

ms.service: storage
ms.topic: how-to
ms.date: 03/28/2022

ms.subservice: blobs
ms.devlang: csharp
ms.custom: devx-track-csharp, devguide-csharp
---

# List blobs with .NET

This article shows how to list blobs using the [Azure Storage client library for .NET](/dotnet/api/overview/azure/storage).

When you list blobs from your code, you can specify a number of options to manage how results are returned from Azure Storage. You can specify the number of results to return in each set of results, and then retrieve the subsequent sets. You can specify a prefix to return blobs whose names begin with that character or string. And you can list blobs in a flat listing structure, or hierarchically. A hierarchical listing returns blobs as though they were organized into folders.

## Understand blob listing options

To list the blobs in a storage account, call one of these methods:

- [BlobContainerClient.GetBlobs](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobs)
- [BlobContainerClient.GetBlobsAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsasync)
- [BlobContainerClient.GetBlobsByHierarchy](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchy)
- [BlobContainerClient.GetBlobsByHierarchyAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchyasync)

### Manage how many results are returned

By default, a listing operation returns up to 5000 results at a time, but you can specify the number of results that you want each listing operation to return. The examples presented in this article show you how to return results in pages.

### Filter results with a prefix

To filter the list of blobs, specify a string for the `prefix` parameter. The prefix string can include one or more characters. Azure Storage then returns only the blobs whose names start with that prefix.

### Return metadata

You can return blob metadata with the results by specifying the **Metadata** value for the [BlobTraits](/dotnet/api/azure.storage.blobs.models.blobtraits) enumeration.

### Flat listing versus hierarchical listing

Blobs in Azure Storage are organized in a flat paradigm, rather than a hierarchical paradigm (like a classic file system). However, you can organize blobs into *virtual directories* in order to mimic a folder structure. A virtual directory forms part of the name of the blob and is indicated by the delimiter character.

To organize blobs into virtual directories, use a delimiter character in the blob name. The default delimiter character is a forward slash (/), but you can specify any character as the delimiter.

If you name your blobs using a delimiter, then you can choose to list blobs hierarchically. For a hierarchical listing operation, Azure Storage returns any virtual directories and blobs beneath the parent object. You can call the listing operation recursively to traverse the hierarchy, similar to how you would traverse a classic file system programmatically.

## Use a flat listing

By default, a listing operation returns blobs in a flat listing. In a flat listing, blobs are not organized by virtual directory.

The following example lists the blobs in the specified container using a flat listing, with an optional segment size specified, and writes the blob name to a console window.

If you've enabled the hierarchical namespace feature on your account, directories are not virtual. Instead, they are concrete, independent objects. Therefore, directories appear in the list as zero-length blobs.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CRUD.cs" id="Snippet_ListBlobsFlatListing":::

The sample output is similar to:

```console
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

## Use a hierarchical listing

When you call a listing operation hierarchically, Azure Storage returns the virtual directories and blobs at the first level of the hierarchy.

To list blobs hierarchically, call the [BlobContainerClient.GetBlobsByHierarchy](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchy), or the [BlobContainerClient.GetBlobsByHierarchyAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchyasync) method.

The following example lists the blobs in the specified container using a hierarchical listing, with an optional segment size specified, and writes the blob name to the console window.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CRUD.cs" id="Snippet_ListBlobsHierarchicalListing":::

The sample output is similar to:

```console
Virtual directory prefix: FolderA/
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt

Virtual directory prefix: FolderA/FolderB/
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt

Virtual directory prefix: FolderA/FolderB/FolderC/
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

> [!NOTE]
> Blob snapshots cannot be listed in a hierarchical listing operation.

### List blob versions or snapshots

To list blob versions or snapshots, specify the [BlobStates](/dotnet/api/azure.storage.blobs.models.blobstates) parameter with the **Version** or **Snapshot** field. Versions and snapshots are listed from oldest to newest. 

The following code example shows how to list blob versions.

```csharp
private static void ListBlobVersions(BlobContainerClient blobContainerClient, 
                                           string blobName)
{
    // Call the listing operation, specifying that blob versions are returned.
    // Use the blob name as the prefix. 
    var blobVersions = blobContainerClient.GetBlobs
        (BlobTraits.None, BlobStates.Version, prefix: blobName)
        .OrderByDescending(version => version.VersionId);

    // Construct the URI for each blob version.
    foreach (var version in blobVersions)
    {
        BlobUriBuilder blobUriBuilder = new BlobUriBuilder(blobContainerClient.Uri)
        {
            BlobName = version.Name,
            VersionId = version.VersionId
        };

        if ((bool)version.IsLatestVersion.GetValueOrDefault())
        {
            Console.WriteLine("Current version: {0}", blobUriBuilder);
        }
        else
        {
            Console.WriteLine("Previous version: {0}", blobUriBuilder);
        }
    }
}
```

## Next steps

- [List Blobs](/rest/api/storageservices/list-blobs)
- [Enumerating Blob Resources](/rest/api/storageservices/enumerating-blob-resources)
- [Blob versioning](versioning-overview.md)
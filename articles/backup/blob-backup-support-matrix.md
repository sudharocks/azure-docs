---
title: Support matrix for Azure Blobs backup
description: Provides a summary of support settings and limitations when backing up Azure Blobs.
ms.topic: conceptual
ms.date: 10/07/2021
ms.custom: references_regions
author: jyothisuri
ms.author: jsuri
---

# Support matrix for Azure Blobs backup

This article summarizes the regional availability, supported scenarios, and limitations of operational backup of blobs.

## Supported regions

Operational backup for blobs is available in all public cloud regions, except France South and South Africa West. It's also available in sovereign cloud regions - all Azure Government regions and China regions (except China East).

## Limitations

Operational backup of blobs uses blob point-in-time restore, blob versioning, soft delete for blobs, change feed for blobs and delete lock to provide a local backup solution. So limitations that apply to these capabilities also apply to operational backup.

**Supported scenarios:** Operational backup supports block blobs in standard general-purpose v2 storage accounts only. Storage accounts with hierarchical namespace enabled (that is, ADLS Gen2 accounts) aren't supported.   <br><br>   Also, any page blobs, append blobs, and premium blobs in your storage account won't be restored and only block blobs will be restored.

**Other limitations:**

- If you've deleted a container during the retention period, that container won't be restored with the point-in-time restore operation. If you attempt to restore a range of blobs that includes blobs in a deleted container, the point-in-time restore operation will fail. For more information about protecting containers from deletion, see [Soft delete for containers](../storage/blobs/soft-delete-container-overview.md).
- If a blob has moved between the hot and cool tiers in the period between the present moment and the restore point, the blob is restored to its previous tier. Restoring block blobs in the archive tier isn't supported. For example, if a blob in the hot tier was moved to the archive tier two days ago, and a restore operation restores to a point three days ago, the blob isn't restored to the hot tier. To restore an archived blob, first move it out of the archive tier. For more information, see [Rehydrate blob data from the archive tier](../storage/blobs/archive-rehydrate-overview.md).
- A block that has been uploaded via [Put Block](/rest/api/storageservices/put-block) or [Put Block from URL](/rest/api/storageservices/put-block-from-url), but not committed via [Put Block List](/rest/api/storageservices/put-block-list), isn't part of a blob and so isn't restored as part of a restore operation.
- A blob with an active lease can't be restored. If a blob with an active lease is included in the range of blobs to restore, the restore operation will fail automatically. Break any active leases before starting the restore operation.
- Snapshots aren't created or deleted as part of a restore operation. Only the base blob is restored to its previous state.
- If there're [immutable blobs](../storage/blobs/immutable-storage-overview.md#about-immutable-storage-for-blobs) among those being restored, such immutable blobs won't be restored to their state as per the selected recovery point. However, other blobs that don't have immutability enabled will be restored to the selected recovery point as expected.

## Next steps

[Overview of operational backup for Azure Blobs](blob-backup-overview.md)

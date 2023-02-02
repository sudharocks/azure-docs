---
title: Azure Stack Edge virtual machine sizing
description: Learn about the VMs that Azure Private 5G Core uses when running on an Azure Stack Edge device.
author: b-branco
ms.author: biancabranco
ms.service: private-5g-core
ms.topic: reference
ms.date: 01/27/2023
---

# Azure Stack Edge virtual machine sizing

The following table contains information about the VMs that Azure Private 5G Core (AP5GC) uses when running on an Azure Stack Edge (ASE) device. You can use this information, for example, to check how much space you'll have remaining on your ASE device after installing Azure Private 5G Core.

| VM detail | Flavor name | vCPUs | Memory (GB) | Disk size | VM function |
|---|---|---|---|---|---|
| Management Control Plane VM | Standard_F4s_v1 | 4 | 4 | Ephemeral - 80 GB | Management Control Plane to create Kubernetes clusters |
| AP5GC Cluster Control Plane VM | Standard_F4s_v1 | 4 | 4 | Ephemeral - 128 GB | Master Node of the Kubernetes cluster used for AP5GC |
| AP5GC Cluster Worker Node VM | Standard_F16s_HPN | 16 | 32 | Ephemeral - 128 GB <br>Persistent - 102 GB | Worker node - AP5GC workload |
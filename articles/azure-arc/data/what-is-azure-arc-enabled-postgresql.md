---
title: What is Azure Arc-enabled PostgreSQL server?
description: What is Azure Arc-enabled PostgreSQL server?
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data-postgresql
author: dhanmm
ms.author: dhmahaja
ms.reviewer: mikeray
ms.date: 11/03/2021
ms.topic: how-to
---

# What is Azure Arc-enabled PostgreSQL server

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]



**Azure Arc-enabled PostgreSQL server** is one of the database engines available as part of Azure Arc-enabled data services. 

## Compare Postgres solutions provided by Microsoft in Azure

Microsoft offers Postgres database services in Azure in two ways:
- As a managed service in **[Azure PaaS](https://portal.azure.com/#create/Microsoft.PostgreSQLServer)** (Platform As A Service)
- As a semi-managed service with Azure Arc as it is operated by customers or their partners/vendors

### Features

- Manage Postgres simply
- Simplify monitoring, back up, patching/upgrade, access control & more
- Deploy Postgres on any [Kubernetes](https://kubernetes.io/) infrastructure
    - On-premises
    - Cloud providers like AWS, GCP, and Azure
    - Edge deployments (including lightweight Kubernetes [K3S](https://k3s.io/))
- Integrate with Azure (optional)
    - Direct connectivity mode - Deploy Azure Arc-enabled PostgreSQL server from the Azure portal
    - Indirect connectivity mode - Deploy Azure Arc-enabled PostgreSQL server from the infrastructure that hosts it
- Pay for what you use (per usage billing)
- Get support from Microsoft on PostgreSQL

## Architecture

Azure Arc-enabled PostgreSQL server is the community version of the [PostgreSQL 14](https://www.postgresql.org/) server with a curated set of available extensions. Most PostgreSQL applications workloads should be capable of running against Azure Arc-enabled PostgreSQL server using standard drivers.


## Next steps

### Try it out

Get started quickly with [Azure Arc Jumpstart](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) on Azure Kubernetes Service (AKS), AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) or in an Azure VM.

### Deploy

Follow these steps to create on your own Kubernetes cluster:
- [Install the client tools](install-client-tools.md)
- [Plan an Azure Arc-enabled data services deployment](plan-azure-arc-data-services.md)
- [Create an Azure Arc-enabled PostgreSQL server on Azure Arc](create-postgresql-server.md) 

### Learn
- [Azure Arc](https://aka.ms/azurearc)
- [Azure Arc-enabled Data Services overview](overview.md)
- [Azure Arc Hybrid Data Services](https://azure.microsoft.com/services/azure-arc/hybrid-data-services)
- [Connectivity modes](connectivity.md)

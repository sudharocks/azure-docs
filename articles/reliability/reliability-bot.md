---
title: Reliability in Azure Bot Service 
description: Find out about reliability in Azure Bot Service  
author: hibrenda 
ms.author: anaharris
ms.topic: overview
ms.custom: subject-reliability
ms.service: bot-service
ms.date: 01/06/2022 
---


# What is reliability in Azure Bot Service?

When you create an application (bot) in Azure, you can choose whether or not your bot resource will have global or local data residency. Local data residency ensures that your bot's personal data is preserved, stored, and processed within certain geographic boundaries (like EU boundaries).

>[!IMPORTANT]
>Availability zone support is not enabled for any standard channels in the regional bot service.

This article describes reliability support in Azure Bot Service, and covers both regional reliability with availability zones and cross-region resiliency with disaster recovery for bots with local data residency. For a more detailed overview of reliability in Azure, see [Azure reliability](/azure/architecture/framework/resiliency/overview).

For more information on deploying bots with local data residency and regional compliance, see [Regionalization in Azure Bot Service](/azure/bot-service/bot-builder-concept-regionalization).

## Availability zone support

Azure availability zones are at least three physically separate groups of datacenters within each Azure region. Datacenters within each zone are equipped with independent power, cooling, and networking infrastructure. In the case of a local zone failure, availability zones are designed so that if the one zone is affected, regional services, capacity, and high availability are supported by the remaining two zones.  Failures can range from software and hardware failures to events such as earthquakes, floods, and fires. Tolerance to failures is achieved with redundancy and logical isolation of Azure services. For more detailed information on availability zones in Azure, see [Regions and availability zones](/azure/availability-zones/az-overview).

For regional bots, Azure Bot Service supports zone redundancy by default.  You don't need to set it up or reconfigure for availability zone support. 

### Prerequisites

- Your bot must be regional (not global). 
- Currently, only the "westeurope" region supports availability zones.

### Zone down experience

During a zone-wide outage, the customer should expect a brief degradation of performance, until the service's self-healing re-balances underlying capacity to adjust to healthy zones. This is not dependent on zone restoration; it is expected that the Microsoft-managed service self-healing state will compensate for a lost zone, leveraging capacity from other zones.


### Cross-region disaster recovery in multi-region geography

Azure Bot Service runs in active-active mode for both global and regional services. When an outage occurs, you don't need to detect errors or manage the service. Azure Bot Service automatically performs auto-failover and auto recovery in a multi-region geographical architecture. For the EU bot regional service, Azure Bot Service provides two full regions inside Europe with active/active replication to ensure redundancy. For the global bot service, all available regions/geographies can be served as the global footprint.


## Next steps

> [!div class="nextstepaction"]
> [Resiliency in Azure](/azure/availability-zones/overview)

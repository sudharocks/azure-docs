---
title: Prepare to deploy a private mobile network
titleSuffix: Azure Private 5G Core
description: Learn how to complete the prerequisite tasks for deploying a private mobile network with Azure Private 5G Core.
author: djrmetaswitch
ms.author: drichards
ms.service: private-5g-core
ms.topic: how-to 
ms.date: 01/31/2023
ms.custom: template-how-to
---

# Complete the prerequisite tasks for deploying a private mobile network

In this how-to guide, you'll carry out each of the tasks you need to complete before you can deploy a private mobile network using Azure Private 5G Core.

## Get access to Azure Private 5G Core for your Azure subscription

Contact your trials engineer and ask them to register your Azure subscription for access to Azure Private 5G Core. If you don't already have a trials engineer and are interested in trialing Azure Private 5G Core, contact your Microsoft account team, or express your interest through the [partner registration form](https://aka.ms/privateMECMSP).

## Choose the core technology type (5G or 4G)

Choose whether each site in the private mobile network should provide coverage for 5G or 4G user equipment (UEs). A single site can't support 5G and 4G UEs simultaneously. If you're deploying multiple sites, you can choose to have some sites support 5G UEs and others support 4G UEs.

## Allocate subnets and IP addresses

Azure Private 5G Core requires a management network, access network, and one or more data networks. These networks can all be part of the same, larger network, or they can be separate. The approach you use depends on your traffic separation requirements.

For each of these networks, allocate a subnet and then identify the listed IP addresses. If you're deploying multiple sites, you'll need to collect this information for each site.

Depending on your networking requirements (for example, if a limited set of subnets is available), you may choose to allocate a single subnet for all of the Azure Stack Edge interfaces, marked with an asterisk (*) in the following list.

### Management network

- Network address in Classless Inter-Domain Routing (CIDR) notation. 
- Default gateway. 
- One IP address for the Azure Stack Edge Pro device's management port. You'll choose a port between 2 and 4 to use as the management port as part of [setting up your Azure Stack Edge Pro device](#order-and-set-up-your-azure-stack-edge-pro-devices).*
- Six sequential IP addresses for the Azure Kubernetes Service on Azure Stack HCI (AKS-HCI) cluster nodes.
- One IP address for accessing local monitoring tools for the packet core instance.

### Access network

- Network address in CIDR notation. 
- Default gateway. 
- One IP address for the control plane interface. For 5G, this interface is the N2 interface, whereas for 4G, it's the S1-MME interface.*
- One IP address for the user plane interface. For 5G, this interface is the N3 interface, whereas for 4G, it's the S1-U interface.*
- One IP address for port 5 on the Azure Stack Edge Pro device.

### Data networks

Allocate the following IP addresses for each data network in the site:

- Network address in CIDR notation.
- Default gateway.
- One IP address for the user plane interface. For 5G, this interface is the N6 interface, whereas for 4G, it's the SGi interface.*

The following IP address must be shared by all the data networks in the site:

- One IP address for port 6 on the Azure Stack Edge Pro device.

## Allocate user equipment (UE) IP address pools

Azure Private 5G Core supports the following IP address allocation methods for UEs.

- Dynamic. Dynamic IP address allocation automatically assigns a new IP address to a UE each time it connects to the private mobile network. 

- Static. Static IP address allocation ensures that a UE receives the same IP address every time it connects to the private mobile network. This is useful when you want Internet of Things (IoT) applications to be able to consistently connect to the same device. For example, you may configure a video analysis application with the IP addresses of the cameras providing video streams. If these cameras have static IP addresses, you won't need to reconfigure the video analysis application with new IP addresses each time the cameras restart. You'll allocate static IP addresses to a UE as part of [provisioning its SIM](provision-sims-azure-portal.md).

You can choose to support one or both of these methods for each data network in your site.

For each data network you're deploying, do the following:

- Decide which IP address allocation methods you want to support.
- For each method you want to support, identify an IP address pool from which IP addresses can be allocated to UEs. You'll need to provide each IP address pool in CIDR notation.

    If you decide to support both methods for a particular data network, ensure that the IP address pools are of the same size and don't overlap.

- Decide whether you want to enable Network Address and Port Translation (NAPT) for the data network. NAPT allows you to translate a large pool of private IP addresses for UEs to a small number of public IP addresses. The translation is performed at the point where traffic enters the data network, maximizing the utility of a limited supply of public IP addresses.

## Configure Domain Name System (DNS) servers

> [!IMPORTANT]
> If you don't configure DNS servers for a data network, all UEs using that network will be unable to resolve domain names.

DNS allows the translation between human-readable domain names and their associated machine-readable IP addresses. Depending on your requirements, you have the following options for configuring a DNS server for your data network:

- If you need the UEs connected to this data network to resolve domain names, you must configure one or more DNS servers. You must use a private DNS server if you need DNS resolution of internal hostnames. If you're only providing internet access to public DNS names, you can use a public or private DNS server.
- If you don't need the UEs to perform DNS resolution, or if all UEs in the network will use their own locally configured DNS servers (instead of the DNS servers signaled to them by the packet core), you can omit this configuration.

## Prepare your networks

For each site you're deploying, do the following. 

- Ensure you have at least one network switch with at least three ports available. You'll connect each Azure Stack Edge Pro device to the switch(es) in the same site as part of the instructions in [Order and set up your Azure Stack Edge Pro device(s)](#order-and-set-up-your-azure-stack-edge-pro-devices).
- For every network where you decided not to enable NAPT (as described in [Allocate user equipment (UE) IP address pools](#allocate-user-equipment-ue-ip-address-pools)), configure the data network to route traffic destined for the UE IP address pools via the IP address you allocated to the packet core instance's user plane interface on the data network.

### Configure ports for local access

The following table contains the ports you need to open for Azure Private 5G Core local access. This includes local management access and control plane signaling.

You must set these up in addition to the [ports required for Azure Stack Edge (ASE)](../databox-online/azure-stack-edge-gpu-system-requirements.md#networking-port-requirements).

| Port | ASE interface | Description|
|--|--|--|
| TCP 443 Inbound      | Management (LAN)        | Access to local monitoring tools (packet core dashboards and distributed tracing). |
| SCTP 38412 Inbound   | Port 5 (Access network) | Control plane access signaling (N2 interface). </br>Only required for 5G deployments. |
| SCTP 36412 Inbound   | Port 5 (Access network) | Control plane access signaling (S1-MME interface). </br>Only required for 4G deployments. |
| UDP 2152 In/Outbound | Port 5 (Access network) | Access network user plane data (N3 interface for 5G, S1-U for 4G). |
| All IP traffic       | Port 6 (Data networks)   | Data network user plane data (N6 interface for 5G, SGi for 4G). |


## Register resource providers and features

To use Azure Private 5G Core, you need to register some additional resource providers and features with your Azure subscription.

> [!TIP]
> If you do not have the Azure CLI installed, see installation instructions at [How to install the Azure CLI](/cli/azure/install-azure-cli). Alternatively, you can use the Azure Cloud Shell on the portal.
    
1. Sign into the Azure CLI with a user account that is associated with the Azure tenant that you are deploying Azure Private 5G Core into:
    ```azurecli-interactive
    az login
    ```
    > [!TIP]
    > See [Sign in interactively](/cli/azure/authenticate-azure-cli) for full instructions.    
1. If your account has multiple subscriptions, make sure you are in the correct one:
    ```azurecli-interactive
    az account set –-subscription <subscription_id>
    ```
1. Check the Azure CLI version:
    ```azurecli-interactive
    az version
    ```
    If the CLI version is below 2.37.0, you will need to upgrade your Azure CLI to a newer version. See [How to update the Azure CLI](/cli/azure/update-azure-cli).
1. Register the following resource providers:
    ```azurecli-interactive
    az provider register --namespace Microsoft.MobileNetwork
    az provider register --namespace Microsoft.HybridNetwork
    az provider register --namespace Microsoft.ExtendedLocation
    az provider register --namespace Microsoft.Kubernetes
    az provider register --namespace Microsoft.KubernetesConfiguration
    ```
1. Register the following features:
    ```azurecli-interactive
    az feature register --name allowVnfCustomer --namespace Microsoft.HybridNetwork
    az feature register --name previewAccess --namespace  Microsoft.Kubernetes
    az feature register --name sourceControlConfiguration --namespace  Microsoft.KubernetesConfiguration
    az feature register --name extensions --namespace  Microsoft.KubernetesConfiguration
    az feature register --name CustomLocations-ppauto --namespace  Microsoft.ExtendedLocation
    ```

## Retrieve the Object ID (OID)

You need to obtain the object ID (OID) of the custom location resource provider in your Azure tenant.  You will need to provide this OID when you configure your ASE to use AKS-HCI.  You can obtain the OID using the Azure CLI. You will need to be an owner of your Azure subscription.

You can retrieve the OID using the Azure CLI:

```azurecli-interactive
az ad sp show --id bc313c14-388c-4e7d-a58e-70017303ee3b --query id -o tsv
```

This command queries the custom location and will output an OID string.  Save this string for use later when you're commissioning the Azure Stack Edge device.

## Order and set up your Azure Stack Edge Pro device(s)

Do the following for each site you want to add to your private mobile network. Detailed instructions for how to carry out each step are included in the **Detailed instructions** column where applicable.

| Step No. | Description | Detailed instructions |
|--|--|--|
| 1. | Complete the Azure Stack Edge Pro deployment checklist.| [Deployment checklist for your Azure Stack Edge Pro GPU device](../databox-online/azure-stack-edge-gpu-deploy-checklist.md)|
| 2. | Order and prepare your Azure Stack Edge Pro device. | [Tutorial: Prepare to deploy Azure Stack Edge Pro with GPU](../databox-online/azure-stack-edge-gpu-deploy-prep.md?tabs=azure-portal) |
| 3. | Rack and cable your Azure Stack Edge Pro device. </br></br>When carrying out this procedure, you must ensure that the device has its ports connected as follows:</br></br>- Port 5 - access network</br>- Port 6 - data networks</br></br>Additionally, you must have a port connected to your management network. You can choose any port from 2 to 4. | [Tutorial: Install Azure Stack Edge Pro with GPU](../databox-online/azure-stack-edge-gpu-deploy-install.md) |
| 4. | Connect to your Azure Stack Edge Pro device using the local web UI. | [Tutorial: Connect to Azure Stack Edge Pro with GPU](../databox-online/azure-stack-edge-gpu-deploy-connect.md) |
| 5. | Configure the network for your Azure Stack Edge Pro device. When carrying out the *Enable compute network* step of this procedure, ensure you use the port you've connected to your management network. </br></br>**Do not** configure Advanced Networks. | [Tutorial: Configure network for Azure Stack Edge Pro with GPU](../databox-online/azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md) |
| 6. | Configure a name, DNS name, and (optionally) time settings. </br></br>**Do not** configure an update. | [Tutorial: Configure the device settings for Azure Stack Edge Pro with GPU](../databox-online/azure-stack-edge-gpu-deploy-set-up-device-update-time.md) |
| 7. | Configure certificates for your Azure Stack Edge Pro device. After changing the certificates, you may have to reopen the local UI in a new browser window to prevent the old cached certificates from causing problems.| [Tutorial: Configure certificates for your Azure Stack Edge Pro with GPU](../databox-online/azure-stack-edge-gpu-deploy-configure-certificates.md) |
| 8. | Activate your Azure Stack Edge Pro device. </br></br>**Do not** follow the section to *Deploy Workloads*. | [Tutorial: Activate Azure Stack Edge Pro with GPU](../databox-online/azure-stack-edge-gpu-deploy-activate.md) |
| 9. | Enable VM management from the Azure portal. </br></br>Enabling this immediately after activating the Azure Stack Edge Pro device occasionally causes an error. Wait one minute and retry.   | Navigate to the ASE resource in the Azure portal, go to **Edge services**, select **Virtual machines** and select **Enable**.   |
| 10. | Run the diagnostics tests for the Azure Stack Edge Pro device in the local web UI, and verify they all pass. </br></br>You may see a warning about a disconnected, unused port. You should fix the issue if the warning relates to any of these ports:</br></br>- Port 5.</br>- Port 6.</br>- The port you chose to connect to the management network in Step 3.</br></br>For all other ports, you can ignore the warning. </br></br>If there are any errors, resolve them before continuing with the remaining steps. This includes any errors related to invalid gateways on unused ports. In this case, either delete the gateway IP address or set it to a valid gateway for the subnet. | [Run diagnostics, collect logs to troubleshoot Azure Stack Edge device issues](../databox-online/azure-stack-edge-gpu-troubleshoot.md) |

> [!IMPORTANT]
> You must ensure your Azure Stack Edge Pro device is compatible with the Azure Private 5G Core version you plan to install. See [Packet core and Azure Stack Edge (ASE) compatibility](/azure/private-5g-core/azure-stack-edge-packet-core-compatibility). If you need to upgrade your Azure Stack Edge Pro device, see [Update your Azure Stack Edge Pro GPU](/azure/databox-online/azure-stack-edge-gpu-install-update?tabs=version-2106-and-later).

## Next steps

You can now commission the Azure Kubernetes Service (AKS) cluster on your Azure Stack Edge Pro device to get it ready to deploy Azure Private 5G Core.

- [Commission an AKS cluster](commission-cluster.md)

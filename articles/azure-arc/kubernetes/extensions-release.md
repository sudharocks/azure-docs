---
title: "Available extensions for Azure Arc-enabled Kubernetes clusters"
ms.date: 01/23/2023
ms.topic: how-to
description: "See which extensions are currently available for Azure Arc-enabled Kubernetes clusters and view release notes."
---

# Available extensions for Azure Arc-enabled Kubernetes clusters

[Cluster extensions for Azure Arc-enabled Kubernetes](conceptual-extensions.md) provide an Azure Resource Manager-driven experience for installation and lifecycle management of different Azure capabilities on top of your cluster. These extensions can be [deployed to your clusters](extensions.md) to enable different scenarios and improve cluster management.

The following extensions are currently available for use with Arc-enabled Kubernetes clusters. All of these extensions are [cluster-scoped](conceptual-extensions.md#extension-scope), except for Azure API Management on Azure Arc, which is namespace-scoped.

> [!NOTE]
> Installing Azure Arc extensions on [Azure Kubernetes Service (AKS) hybrid clusters provisioned from Azure](extensions.md#aks-hybrid-clusters-provisioned-from-azure-preview) is currently in preview, with support for the Azure Arc-enabled Open Service Mesh, Azure Key Vault Secrets Provider, Flux (GitOps) and Microsoft Defender for Cloud extensions.

## Azure Monitor Container Insights

Azure Monitor Container Insights provides visibility into the performance of workloads deployed on the Kubernetes cluster. Use this extension to collect memory and CPU utilization metrics from controllers, nodes, and containers.

For more information, see [Azure Monitor Container Insights for Azure Arc-enabled Kubernetes clusters](../../azure-monitor/containers/container-insights-enable-arc-enabled-clusters.md?toc=/azure/azure-arc/kubernetes/toc.json&bc=/azure/azure-arc/kubernetes/breadcrumb/toc.json).

## Azure Policy

Azure Policy extends [Gatekeeper](https://github.com/open-policy-agent/gatekeeper), an admission controller webhook for [Open Policy Agent](https://www.openpolicyagent.org/) (OPA), to apply at-scale enforcements and safeguards on your clusters in a centralized, consistent manner.

For more information, see [Understand Azure Policy for Kubernetes clusters](../../governance/policy/concepts/policy-for-kubernetes.md?toc=/azure/azure-arc/kubernetes/toc.json&bc=/azure/azure-arc/kubernetes/breadcrumb/toc.json).

## Azure Key Vault Secrets Provider

The Azure Key Vault Provider for Secrets Store CSI Driver allows for the integration of Azure Key Vault as a secrets store with a Kubernetes cluster via a CSI volume. For Azure Arc-enabled Kubernetes clusters, you can install the Azure Key Vault Secrets Provider extension to fetch secrets.

For more information, see [Use the Azure Key Vault Secrets Provider extension to fetch secrets into Azure Arc-enabled Kubernetes clusters](tutorial-akv-secrets-provider.md).

## Microsoft Defender for Containers

Microsoft Defender for Containers is the cloud-native solution that is used to secure your containers so you can improve, monitor, and maintain the security of your clusters, containers, and their applications. It gathers information related to security like audit log data from the Kubernetes cluster, and provides recommendations and threat alerts based on gathered data.

For more information, see [Enable Microsoft Defender for Containers](../../defender-for-cloud/defender-for-kubernetes-azure-arc.md?toc=/azure/azure-arc/kubernetes/toc.json&bc=/azure/azure-arc/kubernetes/breadcrumb/toc.json).

> [!IMPORTANT]
> Defender for Containers support for Arc-enabled Kubernetes clusters is currently in public preview.
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

## Azure Arc-enabled Open Service Mesh

[Open Service Mesh (OSM)](https://docs.openservicemesh.io/) is a lightweight, extensible, Cloud Native service mesh that allows users to uniformly manage, secure, and get out-of-the-box observability features for highly dynamic microservice environments.

For more information, see [Azure Arc-enabled Open Service Mesh](tutorial-arc-enabled-open-service-mesh.md).

## Azure Arc-enabled Data Services

Makes it possible for you to run Azure data services on-premises, at the edge, and in public clouds using Kubernetes and the infrastructure of your choice. This extension enables the *custom locations* feature, providing a way to configure Azure Arc-enabled Kubernetes clusters as target locations for deploying instances of Azure offerings.

For more information, see [Azure Arc-enabled Data Services](../data/create-data-controller-direct-prerequisites.md) and [Create custom locations](custom-locations.md#create-custom-location).

## Azure App Service on Azure Arc

Allows you to provision an App Service Kubernetes environment on top of Azure Arc-enabled Kubernetes clusters.

For more information, see [App Service, Functions, and Logic Apps on Azure Arc (Preview)](../../app-service/overview-arc-integration.md).

> [!IMPORTANT]
> App Service on Azure Arc is currently in public preview. Review the [public preview limitations for App Service Kubernetes environments](../../app-service/overview-arc-integration.md#public-preview-limitations) before deploying this extension.
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

## Azure Event Grid on Kubernetes

Event Grid is an event broker used to integrate workloads that use event-driven architectures. This extension lets you create and manage Event Grid resources such as topics and event subscriptions on top of Azure Arc-enabled Kubernetes clusters.

For more information, see [Event Grid on Kubernetes with Azure Arc (Preview)](../../event-grid/kubernetes/overview.md).

> [!IMPORTANT]
> Event Grid on Kubernetes with Azure Arc is currently in public preview.
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

## Azure API Management on Azure Arc

With the integration between Azure API Management and Azure Arc on Kubernetes, you can deploy the API Management gateway component as an extension in an Azure Arc-enabled Kubernetes cluster. This extension is [namespace-scoped](conceptual-extensions.md#extension-scope), not cluster-scoped.

For more information, see [Deploy an Azure API Management gateway on Azure Arc (preview)](../../api-management/how-to-deploy-self-hosted-gateway-azure-arc.md).

> [!IMPORTANT]
> API Management self-hosted gateway on Azure Arc is currently in public preview. During preview, the API Management gateway extension is available in the following regions: West Europe, East US.
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

## Azure Arc-enabled Machine Learning

The AzureML extension lets you deploy and run Azure Machine Learning on Azure Arc-enabled Kubernetes clusters.

For more information, see [Introduction to Kubernetes compute target in AzureML](../../machine-learning/how-to-attach-kubernetes-anywhere.md) and [Deploy AzureML extension on AKS or Arc Kubernetes cluster](../../machine-learning/how-to-deploy-kubernetes-extension.md).

## Flux (GitOps)

[GitOps on Azure Arc-enabled Kubernetes](conceptual-gitops-flux2.md) uses [Flux v2](https://fluxcd.io/docs/), a popular open-source tool set, to help manage cluster configuration and application deployment. GitOps is enabled in the cluster as a `Microsoft.KubernetesConfiguration/extensions/microsoft.flux` cluster extension resource.

For more information, see [Tutorial: Deploy applications using GitOps with Flux v2](tutorial-use-gitops-flux2.md).

The currently supported versions of the `microsoft.flux` extension are described below. The most recent version of the Flux v2 extension and the two previous versions (N-2) are supported. We generally recommend that you use the most recent version of the extension.

### 1.6.3 (December 2022)

Flux version: [Release v0.37.0](https://github.com/fluxcd/flux2/releases/tag/v0.37.0)

- source-controller: v0.32.1
- kustomize-controller: v0.31.0
- helm-controller: v0.27.0
- notification-controller: v0.29.0
- image-automation-controller: v0.27.0
- image-reflector-controller: v0.23.0

Changes made for this version:

- Upgrades Flux to [v0.37.0](https://github.com/fluxcd/flux2/releases/tag/v0.37.0)
- Adds exception for [aad-pod-identity in flux extension](troubleshooting.md#flux-v2---installing-the-microsoftflux-extension-in-a-cluster-with-azure-ad-pod-identity-enabled)
- Enables reconciler for flux extension

### 1.6.1 (October 2022)

Flux version: [Release v0.35.0](https://github.com/fluxcd/flux2/releases/tag/v0.35.0)

- source-controller: v0.30.1
- kustomize-controller: v0.29.0
- helm-controller: v0.25.0
- notification-controller: v0.27.0
- image-automation-controller: v0.26.0
- image-reflector-controller: v0.22.0

Changes made for this version:

- Upgrades Flux to [v0.35.0](https://github.com/fluxcd/flux2/releases/tag/v0.35.0)
- Implements fix for a security issue where some Flux controllers could be vulnerable to a denial of service attack. Users that have permissions to change Flux's objects, either through a Flux source or directly within a cluster, could provide invalid data to fields `spec.Interval` or `spec.Timeout` (and structured variations of these fields), causing the entire object type to stop being processed. This issue had two root causes: [Kubernetes type `metav1.Duration` not being fully compatible with the Go type `time.Duration`](https://github.com/kubernetes/apimachinery/issues/131), or a lack of validation within Flux to restrict allowed values.
- Adds support for [installing the `microsoft.flux` extension in a cluster with kubelet identity enabled](troubleshooting.md#flux-v2---installing-the-microsoftflux-extension-in-a-cluster-with-kubelet-identity-enabled)
- Fixes bug where [deleting the extension may fail on AKS with Windows node pool](https://github.com/Azure/AKS/issues/3191)
- Adds support for sasToken for Azure blob storage at account level as well as container level

### 1.6.0 (September 2022)

Flux version: [Release v0.33.0](https://github.com/fluxcd/flux2/releases/tag/v0.33.0)

- source-controller: v0.28.0
- kustomize-controller: v0.27.1
- helm-controller: v0.23.1
- notification-controller: v0.25.2
- image-automation-controller: v0.24.2
- image-reflector-controller: v0.20.1

Changes made for this version:

- Upgrades Flux to [v0.33.0](https://github.com/fluxcd/flux2/releases/tag/v0.33.0)
- Fixes Helm-related [security issue](https://github.com/fluxcd/flux2/security/advisories/GHSA-p2g7-xwvr-rrw3)

## Dapr extension for Azure Kubernetes Service (AKS) and Arc-enabled Kubernetes

[Dapr](https://dapr.io/) is a portable, event-driven runtime that simplifies building resilient, stateless, and stateful applications that run on the cloud and edge and embrace the diversity of languages and developer frameworks. The Dapr extension eliminates the overhead of downloading Dapr tooling and manually installing and managing the runtime on your clusters.

For more information, see [Dapr extension for AKS and Arc-enabled Kubernetes](../../aks/dapr.md).

## Next steps

- Read more about [cluster extensions for Azure Arc-enabled Kubernetes](conceptual-extensions.md).
- Learn how to [deploy extensions to an Arc-enabled Kubernetes cluster](extensions.md).

---
title: Deploy the MedTech service using an Azure Resource Manager template - Azure Health Data Services
description: In this article, you'll learn how to deploy the MedTech service using an Azure Resource Manager template.
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 1/20/2023
ms.author: jasteppe
---

# Quickstart: Deploy the MedTech service using an Azure Resource Manager template

To implement infrastructure as code for your Azure solutions, use Azure Resource Manager templates (ARM templates). The template is a JavaScript Object Notation (JSON) file that defines the infrastructure and configuration for your project. The template uses declarative syntax, which lets you state what you intend to deploy without having to write the sequence of programming commands to create it. In the template, you specify the resources to deploy and the properties for those resources. 

In this quickstart, you'll learn how to:

> [!div class="checklist"]
> - Open an ARM template in the Azure portal.
> - Configure the ARM template for your deployment.
> - Deploy the ARM template. 

> [!TIP]
> To learn more about ARM templates, see [What are ARM templates?](./../../azure-resource-manager/templates/overview.md)

## Prerequisites

To begin your deployment and complete the quickstart, you must have the following prerequisites:

- An active Azure subscription account. If you don't have an Azure subscription, see [Subscription decision guide](/azure/cloud-adoption-framework/decision-guides/subscriptions/).

- Owner or Contributor and User Access Administrator role assignments in the Azure subscription. For more information, see [What is Azure role-based access control (Azure RBAC)?](../../role-based-access-control/overview.md)

- The Microsoft.HealthcareApis and Microsoft.EventHub resource providers registered with your Azure subscription. To learn more about registering resource providers, see [Azure resource providers and types](../../azure-resource-manager/management/resource-providers-and-types.md).

When you have these prerequisites, you're ready to configure the ARM template by using the **Deploy to Azure** button.

## Review the ARM template - Optional

The ARM template used to deploy the resources in this quickstart is available at [Azure Quickstart Templates](/samples/azure/azure-quickstart-templates/iotconnectors-with-iothub/) by using the *azuredeploy.json* file on [GitHub](https://github.com/azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.healthcareapis/workspaces/iotconnectors-with-iothub). 

## Use the Deploy to Azure button

To begin deployment in the Azure portal, select the **Deploy to Azure** button:

 [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.healthcareapis%2Fworkspaces%2Fiotconnectors%2Fazuredeploy.json).

## Configure the deployment

1. In the Azure portal, on the Basics tab of the Azure Quickstart Template, select or enter the following information for your deployment:

   - **Subscription** - The Azure subscription to use for the deployment.

   - **Resource group** - An existing resource group, or you can create a new resource group.

   - **Region** - The Azure region of the resource group that's used for the deployment. Region auto-fills by using the resource group region.

   - **Basename** - A value that's appended to the name of the Azure resources and services that are deployed.

   - **Location** - Use the drop-down list to select a supported Azure region for the Azure Health Data Services (the value could be the same or different region than your resource group).

   - **Device Mapping** - Don't change the default values for this quickstart.
  
   - **Destination Mapping** - Don't change the default values for this quickstart.

   :::image type="content" source="media\deploy-new-arm\iot-deploy-quickstart-options.png" alt-text="Screenshot of Azure portal page displaying deployment options for the Azure Health Data Service MedTech service." lightbox="media\deploy-new-arm\iot-deploy-quickstart-options.png":::

2. To validate your configuration, select **Review + create**.

   :::image type="content" source="media\deploy-new-arm\iot-review-and-create-button.png" alt-text="Screenshot that shows the Review + create button selected in the Azure portal.":::

3. In **Review + create**, check the template validation status. If validation is successful, the template displays **Validation Passed**. If validation fails, fix the detail that's indicated in the error message, and then select **Review + create** again.

   :::image type="content" source="media\deploy-new-arm\iot-validation-completed.png" alt-text="Screenshot that shows the Review + create pane displaying the Validation Passed message.":::

4. After a successful validation, to begin the deployment, select **Create**.

   :::image type="content" source="media\deploy-new-arm\iot-create-button.png" alt-text="Screenshot that shows the highlighted Create button.":::

5. In a few minutes, the Azure portal displays the message that your deployment is completed.

   :::image type="content" source="media\deploy-new-arm\iot-deployment-complete-banner.png" alt-text="Screenshot that shows a green checkmark and the message Your deployment is complete.":::

   > [!IMPORTANT]
   > If you're going to allow access from multiple services to the device message event hub, it's required that each service has its own event hub consumer group.
   >
   > Consumer groups enable multiple consuming applications to have a separate view of the event stream, and to read the stream independently at their own pace and with their own offsets. For more information, see [Consumer groups](../../event-hubs/event-hubs-features.md#consumer-groups).
   >
   > Examples:
   >
   > - Two MedTech services accessing the same device message event hub.
   >
   > - A MedTech service and a storage writer application accessing the same device message event hub.

## Review deployed resources and access permissions

When deployment is completed, the following resources and access roles are created in the ARM template deployment:

- Azure Event Hubs namespace and device message event hub. In this deployment, the device message event hub is named *devicedata*.

  - An event hub consumer group. In this deployment, the consumer group is named *$Default*.

  - An Azure Event Hubs Data Sender role. In this deployment, the sender role is named *devicedatasender* and can be used to provide access to the device event hub using a shared access signature (SAS). To learn more about authorizing access using a SAS, see [Authorizing access to Event Hubs resources using Shared Access Signatures](../../event-hubs/authorize-access-shared-access-signature.md).

- A Health Data Services workspace.

- A Health Data Services Fast Healthcare Interoperability Resources (FHIR&#174;) service.

- A Health Data Services MedTech service with the required [system-assigned managed identity](../../active-directory/managed-identities-azure-resources/overview.md) roles:

  - For the device message event hub, the Azure Events Hubs Data Receiver role is assigned in the [Access control section (IAM)](../../role-based-access-control/overview.md) of the device message event hub.

  - For the FHIR service, the FHIR Data Writer role is assigned in the [Access control section (IAM)](../../role-based-access-control/overview.md) of the FHIR service.

> [!IMPORTANT]
> In this quickstart, the ARM template configures the MedTech service to operate in Create mode. A patient resource and a device resource are created for each device that sends data to your FHIR service.
>
> To learn more about the MedTech service resolution types Create and Lookup, see [Destination properties](deploy-new-config.md#destination-properties).

## Post-deployment mappings

After you've successfully deployed an instance of the MedTech service, you'll still need to provide conforming and valid device and FHIR destination mappings.

 - To learn about device mappings, see [How to configure device mappings](how-to-configure-device-mappings.md).

 - To learn about FHIR destination mappings, see [How to configure FHIR destination mappings](how-to-configure-fhir-mappings.md).

## Next steps

In this quickstart, you learned how to deploy an instance of the MedTech service in the Azure portal using an ARM template with a **Deploy to Azure** button. 

To learn about other methods for deploying the MedTech service, see

> [!div class="nextstepaction"]
> [Choose a deployment method for the MedTech service](deploy-new-choose.md)

FHIR&#174; is a registered trademark of Health Level Seven International, registered in the U.S. Trademark Office and is used with their permission.
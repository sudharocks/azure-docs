---
title: Deploy Azure Communications Gateway 
description: This article guides you through how to deploy an Azure Communications Gateway.
author: rcdun
ms.author: rdunstan
ms.service: communications-gateway
ms.topic: how-to 
ms.date: 12/16/2022
---

# Deploy Azure Communications Gateway

This article will guide you through creating an Azure Communications Gateway resource in Azure. You must configure this resource before you can deploy Azure Communications Gateway.

## Prerequisites

Carry out the steps detailed in [Prepare to deploy Azure Communications Gateway](prepare-to-deploy.md).

## 1. Start creating the Azure Communications Gateway resource

In this step, you'll create the Azure Communications Gateway resource.

1. Sign in to the [Azure portal](https://azure.microsoft.com/).
1. In the search bar at the top of the page, search for Communications Gateway and select **Communications Gateways**.  

    :::image type="content" source="media/deploy/search.png" alt-text="Screenshot of the Azure portal. It shows the results of a search for Azure Communications Gateway.":::

1. Select the **Create** option.

    :::image type="content" source="media/deploy/create.png" alt-text="Screenshot of the Azure portal. Shows the existing Azure Communications Gateway. A Create button allows you to create more Azure Communications Gateways.":::

1. Use the information you collected in [Collect Azure Communications Gateway resource values](prepare-to-deploy.md#6-collect-basic-information-for-deploying-an-azure-communications-gateway) to fill out the fields in the **Basics** configuration section and then select **Next: Service Regions**.

    :::image type="content" source="media/deploy/basics.png" alt-text="Screenshot of the Create an Azure Communications Gateway portal, showing the Basics section.":::

1. Use the information you collected in [Collect Service Regions configuration values](prepare-to-deploy.md#7-collect-service-regions-configuration-values) to fill out the fields in the **Service Regions** section and then select **Next: Tags**.
1. (Optional) Configure tags for your Azure Communications Gateway resource: enter a **Name** and **Value** for each tag you want to create.
1. Select **Review + create**.

If you've entered your configuration correctly, you'll see a **Validation Passed** message at the top of your screen. Navigate to the **Review + create** section.

If you haven't filled in the configuration correctly, you'll see an error message in the configuration section(s) containing the invalid configuration. Correct the invalid configuration by selecting the flagged section(s) and use the information within the error messages to correct invalid configuration before returning to the **Review + create** section.

:::image type="content" source="media/deploy/failed-validation.png" alt-text="Screenshot of the Create an Azure Communications Gateway portal, showing a validation that failed due to missing information in the Contacts section.":::

## 2. Submit your Azure Communications Gateway configuration

Check your configuration and ensure it matches your requirements. If the configuration is correct, select **Create**.

You now need to wait for your resource to be provisioned and connected to the Teams environment. Upon completion your onboarding team will reach out to you and the Provisioning Status filed on the resource overview will show as "Complete". We recommend you check in periodically to see if your resource has been provisioned. This process can take up to two weeks as updating ACLs in the Azure and Teams environments is done on a periodic basis.

Once your resource has been provisioned, a message will appear saying **Your deployment is complete**. Select **Go to resource group**, and then check that your resource group contains the correct Azure Communications Gateway resource.

:::image type="content" source="media/deploy/go-to-resource-group.png" alt-text="Screenshot of the Create an Azure Communications Gateway portal, showing a completed deployment screen.":::

## 3. Complete the JSON onboarding file

Your onboarding team will require additional information to complete your Operator Connect onboarding. If you're being onboarded to Operator Connect/Teams Phone Mobile by Microsoft, the onboarding team will reach out to you.
Wait for your onboarding team to confirm that the process is complete before testing your portal access.

## 4. Test your portal access

Navigate to the [Operator Connect homepage](https://operatorconnect.microsoft.com/) and ensure you're able to sign in.

## 5. Register your Fully Qualified Domain Name (FQDN)

Your Azure Communications Gateway will require a custom domain name inside your Active Directory tenant. Follow this step to set up the custom domain name that Teams will use to recognize an Azure Communications Gateway that belongs to you.

1. Navigate to your Azure Communications Gateway resource and select **Properties**. You'll see a field named **Domain name**. This name is your custom domain name.
1. Complete the following procedure: [Add your custom domain name to Azure AD](/azure/active-directory/fundamentals/add-custom-domain).
1. Share your DNS TXT record information with your onboarding team. Wait for your onboarding team to confirm that the DNS TXT record has been configured correctly.
1. Complete the following procedure: [Verify your custom domain name](/azure/active-directory/fundamentals/add-custom-domain).

## Next steps

- [Prepare for live traffic with Azure Communications Gateway](prepare-for-live-traffic.md)

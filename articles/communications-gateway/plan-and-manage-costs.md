---
title: Plan and manage costs for Azure Communications Gateway
description: Learn how to plan for and manage costs for Azure Communications Gateway by using cost analysis in the Azure portal.
author: rcdun
ms.author: rdunstan
ms.custom: subject-cost-optimization
ms.service: communications-gateway
ms.topic: how-to
ms.date: 12/06/2022
---

# Plan and manage costs for Azure Communications Gateway

This article describes how you plan for and manage costs for Azure Communications Gateway.

After you've started using Azure Communications Gateway, you can use Cost Management features to set budgets and monitor costs. You can also review forecasted costs and identify spending trends to identify areas where you might want to act.

Costs for Azure Communications Gateway are only a portion of the monthly costs in your Azure bill. Although this article explains how to plan for and manage costs for Azure Communications Gateway, you're billed for all Azure services and resources used in your Azure subscription. This billing includes third-party services.

## Prerequisites

Cost analysis in Cost Management supports most Azure account types, but not all of them. To view the full list of supported account types, see [Understand Cost Management data](../cost-management-billing/costs/understand-cost-mgt-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn). To view cost data, you need at least read access for an Azure account. For information about assigning access to Azure Cost Management data, see [Assign access to data](../cost-management/assign-access-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## Understand the full billing model for Azure Communications Gateway

Azure Communications Gateway runs on Azure infrastructure that accrues costs when you deploy new resources. It's important to understand that there could be other infrastructure costs that might accrue.

### How you're charged for Azure Communications Gateway

When you deploy or use Azure Communications Gateway, you'll be charged for your use of the voice features of the product. The charges are based on the number of users assigned to the platform by a series of SBC User meters. The meters include:

- A "service availability" meter that is charged hourly and includes the use of 999 users for testing and early adoption.
- A per-user meter that charges based on the number of users that are assigned to the deployment. This per-user fee is calculated from the maximum number of users during your billing cycle, excluding the initial 999 users included in the service availability fee.

If you choose to deploy the API Bridge (for API mediation or the API Bridge Number Management Portal), you'll also be charged for your API Bridge usage. Fees for API Bridge work in the same way as the SBC User meters: a service availability meter and a per-user meter. The number of users charged for the API Bridge is always the same as the number of users charged on the SBC User meters.

> [!NOTE]
> A user is any telephone number that meets all the following criteria.
>
> - You have provisioned the number within your Operator Connect or Teams Phone Mobile environment.
> - The number is configured for connectivity through Azure Communications Gateway.
> - The number's status is "assigned" in the Operator Connect environment. This includes (but is not limited to) assignment to users, Conferencing bridges, Voice Applications and Third Party applications.
>
> Azure Communications Gateway does not charge for Telephone Numbers (TNs) that are not "assigned" in the Operator Connect environment.

At the end of your billing cycle, the charges for each meter are summed. Your bill or invoice shows a section for all Azure Communications Gateway costs. There's a separate line item for each meter.

If you've arranged any custom work with Microsoft, you might be charged an extra fee for that work. That fee isn't included in these meters.

If your Azure subscription has a spending limit, Azure prevents you from spending over your credit amount. As you create and use Azure resources, your credits are used. When you reach your credit limit, the resources that you deployed are disabled for the rest of that billing period. You can't change your credit limit, but you can remove it. For more information about spending limits, see [Azure spending limit](../cost-management-billing/manage/spending-limit.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

### Other costs that might accrue with Azure Communications Gateway

You'll need to pay for Azure networking costs, because these costs aren't included in the Azure Communications Gateway meters.

- If you're connecting to the public internet with Microsoft Azure Peering Service (MAPS), you might need to pay a third party for the cross-connect at the exchange location.
- If you're connecting into Azure as a next hop, you might need to pay vNet peering costs.

### Costs if you cancel or change your deployment

If you cancel Azure Communications Gateway, your final bill or invoice will only include charges on service-availability meters for the part of the billing cycle before you cancel. Per-user meters charge for the entire billing cycle. 

You'll need to remove any networking resources that you set up for Azure Communications Gateway. For example, if you're connecting into Azure as a next hop, you'll need to remove the vNet peering. Otherwise, you'll still be charged for those networking resources.

If you have multiple Azure Communications Gateway deployments and you move users between deployments, these users will count towards meters in both deployments. This double counting only applies to the billing cycle in which you move the subscribers; in the next billing cycle, the users will only count towards meters in their new deployment.

### Using Azure Prepayment with Azure Communications Gateway

You can pay for Azure Communications Gateway charges with your Azure Prepayment credit. However, you can't use Azure Prepayment credit to pay for charges for third party products and services including those from the Azure Marketplace.

## Monitor costs

When you deploy and use Azure Communications Gateway, you incur costs. You can see these costs in [cost analysis](../cost-management/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

When you use cost analysis, you view Azure Communications Gateway costs in graphs and tables for different time intervals. Some examples are by day, current and prior month, and year. You also view costs against budgets and forecasted costs. Switching to longer views over time can help you identify spending trends. And you see where overspending might have occurred. If you've created budgets, you can also easily see where they're exceeded.

To view Azure Communications Gateway costs in cost analysis:

1. Sign in to the Azure portal.
2. Open the scope in the Azure portal and select **Cost analysis** in the menu. For example, go to **Subscriptions**, select a subscription from the list, and then select  **Cost analysis** in the menu. Select **Scope** to switch to a different scope in cost analysis.
3. By default, cost for services are shown in the first donut chart. Select the area in the chart labeled Azure Communications Gateway.

Actual monthly costs are shown when you initially open cost analysis.

To narrow costs for a single service, like Azure Communications Gateway, select **Add filter** and then select **Service name**. Then, select **Azure Communications Gateway**. From here, you can explore costs on your own.

## Create budgets

You can create [budgets](../cost-management/tutorial-acm-create-budgets.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) to manage costs and create [alerts](../cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) that automatically notify stakeholders of spending anomalies and overspending risks. Alerts are based on spending compared to budget and cost thresholds. Budgets and alerts are created for Azure subscriptions and resource groups, so they're useful as part of an overall cost monitoring strategy.

Budgets can be created with filters for specific resources or services in Azure if you want more granularity present in your monitoring. Filters help ensure that you don't accidentally create new resources that cost you additional money. For more information about the filter options available when you create a budget, see [Group and filter options](../cost-management-billing/costs/group-filter.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## Export cost data

You can also [export your cost data](../cost-management-billing/costs/tutorial-export-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) to a storage account. Exporting cost data is helpful when you or others need to do further data analysis. For example, a finance team can analyze the data using Excel or Power BI. You can export your costs on a daily, weekly, or monthly schedule and set a custom date range. Exporting cost data is the recommended way to retrieve cost datasets.

## Next steps

- Learn [how to optimize your cloud investment with Azure Cost Management](../cost-management-billing/costs/cost-mgt-best-practices.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Learn more about managing costs with [cost analysis](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Learn about how to [prevent unexpected costs](../cost-management-billing/understand/analyze-unexpected-charges.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Take the [Cost Management](/training/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) guided learning course.

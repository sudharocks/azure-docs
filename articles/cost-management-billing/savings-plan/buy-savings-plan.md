---
title: Buy an Azure savings plan
titleSuffix: Microsoft Cost Management
description: This article helps you buy an Azure savings plan.
author: bandersmsft
ms.reviewer: onwokolo
ms.service: cost-management-billing
ms.subservice: savings-plan
ms.custom: ignite-2022
ms.topic: how-to
ms.date: 01/30/2023
ms.author: banders
---

# Buy an Azure savings plan

Azure savings plans help you save money by committing to an hourly spend for one-year or three-years plans for Azure compute resources. Saving plans discounts apply to usage from virtual machines, Dedicated Hosts, Container Instances, App Services and Azure Premium Functions. The hourly commitment is priced in USD for Microsoft Customer Agreement customers and local currency for Enterprise customers.

Before you enter a commitment to buy a savings plan, review the following sections to prepare for your purchase.

## Who can buy a savings plan

You can buy a savings plan for an Azure subscription that's of type Enterprise Agreement (EA) offer code MS-AZR-0017P or MS-AZR-0148P, Microsoft Customer Agreement (MCA), or Microsoft Partner Agreement (MPA). If don't know what subscription type you have, see [check your billing type](../manage/view-all-accounts.md#check-the-type-of-your-account).

## Change agreement type to one supported by savings plan

If your current agreement type isn't supported by a savings plan, you might be able to transfer or migrate it to one that's supported. For more information, see the following articles.

- [Transfer Azure products between different billing agreements](../manage/subscription-transfer.md)
- [Product transfer support](../manage/subscription-transfer.md#product-transfer-support)
- [From MOSA to the Microsoft Customer Agreement](https://www.microsoft.com/licensing/news/from-mosa-to-microsoft-customer-agreement)

## Required permission and how to buy

You can buy a savings plan using the Azure portal or with the [Savings Plan Order Alias - Create](/rest/api/billingbenefits/savings-plan-order-alias/create) REST API.

### Purchase in the Azure portal

Required permission and the steps to buy vary, depending on your agreement type.

#### Enterprise Agreement customers

-	EA admins with write permissions can directly purchase savings plans from **Cost Management + Billing** > **Savings plan**. No specific permission for a subscription is needed.
-	Subscription owners for one of the subscriptions in the EA enrollment can purchase savings plans from **Home** > **Savings plan**.
-	Enterprise Agreement (EA) customers can limit purchases to EA admins only by disabling the **Add Savings Plan** option in the [Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade/BillingAccounts). Navigate to the **Policies** menu to change settings. 

#### Microsoft Customer Agreement (MCA) customers

-	Customers with billing profile contributor permissions and above can purchase savings plans from **Cost Management + Billing** > **Savings plan** experience. No specific permissions on a subscription needed.
-	Subscription owners for one of the subscriptions in the billing profile can purchase savings plans from **Home** > **Savings plan**.
-	To disallow savings plan purchases on a billing profile, billing profile contributors can navigate to the Policies menu under the billing profile and adjust **Azure Savings Plan** option.

#### Microsoft Partner Agreement partners

-	Partners can use **Home** > **Savings plan** in the Azure portal to purchase savings plans for their customers.

### Purchase with the Savings Plan Order Alias - Create API

Buy savings plans by using RBAC permissions or with permissions on your billing scope. When using the [Savings Plan Order Alias - Create](/rest/api/billingbenefits/savings-plan-order-alias/create) REST API, the format of the `billingScopeId` in the request body is used to control the permissions that are checked. 

To purchase using RBAC permissions:

- You must be an Owner of the subscription which you plan to use, specified as `billingScopeId`.
- The `billingScopeId` property in the request body must use the `/subscriptions/10000000-0000-0000-0000-000000000000` format.

To purchase using billing permissions:

Permission needed to purchase varies by the type of account that you have.

- For Enterprise agreement customers, you must be an EA admin with write permissions.
- For Microsoft Customer Agreement (MCA) customers, you must be a billing profile contributor or above.
- For Microsoft Partner Agreement partners, only RBAC permissions are currently supported

The `billingScopeId` property in the request body must use the `/providers/Microsoft.Billing/billingAccounts/{accountId}/billingSubscriptions/10000000-0000-0000-0000-000000000000` format.

## Scope savings plans

You can scope a savings plan to a shared scope, management group, subscription, or resource group scopes. Setting the scope for a savings plan selects where the savings plan savings apply. When you scope the savings plan to a resource group, savings plan discounts apply only to the resource group—not the entire subscription.

### Savings plan scoping options

You have the following options to scope a savings plan, depending on your needs:

- **Shared scope** - Applies the savings plan discounts to matching resources in eligible subscriptions that are in the billing scope. If a subscription was moved to a different billing scope, the benefit no longer applies to the subscription. It does continue to apply to other subscriptions in the billing scope.
  - For Enterprise Agreement customers, the billing scope is the enrollment. The savings plan shared scope would include multiple Active Directory tenants in an enrollment.
  - For Microsoft Customer Agreement customers, the billing scope is the billing profile.
  - For Microsoft Partner Agreement, the billing scope is a customer.
- **Single subscription scope** - Applies the savings plan discounts to the matching resources in the selected subscription.
- **Management group** - Applies the savings plan discounts to the matching resource in the list of subscriptions that are a part of both the management group and billing scope. To scope a savings plan to a management group, you must have at least read permission on the management group.
- **Single resource group scope** - Applies the savings plan discounts to the matching resources in the selected resource group only.

When savings plan discounts are applied to your usage, Azure processes the savings plan in the following order:

1. Savings plans with a single resource group scope
2. Savings plans with a single subscription scope
3. Savings plans scoped to a management group
4. Savings plans with a shared scope (multiple subscriptions), described previously

You can always update the scope after you buy a savings plan. To do so, go to the savings plan, select **Configuration**, and rescope the savings plan. Rescoping a savings plan isn't a commercial transaction. Your savings plan term isn't changed. For more information about updating the scope, see [Update the scope after you purchase a savings plan](manage-savings-plan.md#change-the-savings-plan-scope).

:::image type="content" source="./media/buy-savings-plan/scope-savings-plan.png" alt-text="Screenshot showing saving plan scope options." lightbox="./media/buy-savings-plan/scope-savings-plan.png" :::

## Discounted subscription and offer types

Savings plan discounts apply to the following eligible subscriptions and offer types.

- Enterprise agreement (offer numbers: MS-AZR-0017P or MS-AZR-0148P)
- Microsoft Customer Agreement subscriptions.
- Microsoft Partner Agreement subscriptions.

### Buy savings plans with monthly payments

You can pay for savings plans with monthly payments. Unlike an up-front purchase where you pay the full amount, the monthly payment option divides the total cost of the savings plan evenly over each month of the term. The total cost of up-front and monthly savings plans is the same and you don't pay any extra fees when you choose to pay monthly.

If savings plan is purchased using an MCA, your monthly payment amount may vary, depending on the current month's market exchange rate for your local currency.

## View payments made

You can view payments that were made using APIs, usage data, and cost analysis. For savings plans paid for monthly, the frequency value is shown as  **recurring** in the usage data and the Savings Plan Charges API. For savings plans paid up front, the value is shown as **onetime**.

Cost analysis shows monthly purchases in the default view. Apply the **purchase** filter to **Charge type** and **recurring** for **Frequency** to see all purchases. To view only savings plans, apply a filter for **Savings Plan**.

:::image type="content" source="./media/buy-savings-plan/cost-analysis-savings-plan-costs.png" alt-text="Screenshot showing saving plan costs in cost analysis." lightbox="./media/buy-savings-plan/cost-analysis-savings-plan-costs.png" :::

## Reservation trade ins and refunds

Unlike reservations, you can't return or exchange savings plans.

You can trade in one or more reservations for a savings plan. When you trade in reservations, the hourly commitment of the new savings plan must be greater than the leftover payments that are canceled for the returned reservations. There are no other limits or fees for trade ins. You can trade in a reservation that's paid for up front to purchase a new savings plan that's billed monthly. However, the lifetime value of the new savings plan must be greater than the prorated value of the reservations traded in.

## Savings plan notifications

Depending on how you pay for your Azure subscription, email savings plan notifications are sent to the following users in your organization. Notifications are sent for various events including:

- Purchase
- Upcoming savings plan expiration - 30 days before
- Expiry - 30 days before
- Renewal
- Cancellation
- Scope change

For customers with EA subscriptions:

- Notifications are sent to EA administrators and EA notification contacts.
- Azure RBAC owner of the savings plan receives all notifications.

For customers with MCA subscriptions:

- The purchaser receives a purchase notification.
- Azure RBAC owner of the savings plan receives all notifications.

For Microsoft Partner Agreement partners:

- Notifications are sent to the partner.

## Need help? Contact us.

If you have Azure savings plan for compute questions, contact your  account team, or [create a support request](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Temporarily, Microsoft will only provide Azure savings plan for compute expert support requests in English.

## Next steps

- [Permissions to view and manage Azure savings plans](permission-view-manage.md)
- [Manage Azure savings plans](manage-savings-plan.md)

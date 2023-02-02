---
title: Azure Virtual Desktop scaling plans for host pools in Azure Virtual Desktop
description: How to assign scaling plans to new or existing host pools in your deployment.
author: Heidilohr
ms.topic: how-to
ms.date: 08/03/2022
ms.author: helohr
manager: femila
---
# Assign scaling plans to host pools in Azure Virtual Desktop

You can assign a scaling plan for any existing host pools in your deployment. When you apply a scaling plan to your host pool, the plan will apply to all session hosts within that host pool. The scaling plan also automatically applies to any new session hosts you create in your assigned host pool.

If you disable a scaling plan, all assigned resources will remain in the scaling state they were in at the time you disabled it.

## Assign a scaling plan to a single existing host pool

To assign a scaling plan to an existing host pool:

1. Open the [Azure portal](https://portal.azure.com).

1. In the search bar, type *Azure Virtual Desktop* and select the matching service entry.

1. Select **Host pools**, and select the host pool you want to assign the scaling plan to.

1. Under the **Settings** heading, select **Scaling plan**, and then select **+ Assign**. Select the scaling plan you want to assign and select **Assign**. The scaling plan must be in the same Azure region as the host pool.

> [!TIP]
> If you've enabled the scaling plan during deployment, then you'll also have the option to disable the plan for the selected host pool in the **Scaling plan** menu by unselecting the **Enable autoscale** checkbox, as shown in the following screenshot.
>
> [!div class="mx-imgBorder"]
> ![A screenshot of the scaling plan window. The "enable autoscale" check box is selected and highlighted with a red border.](media/enable-autoscale.png)

## Assign a scaling plan to multiple existing host pools

To assign a scaling plan multiple existing host pool at the same time:

1. Open the [Azure portal](https://portal.azure.com).

1. In the search bar, type *Azure Virtual Desktop* and select the matching service entry.

1. Select **Scaling plans**, and select the scaling plan you want to assign to host pools.

1. Under the **Manage** heading, select **Host pool assignments**, and then select **+ Assign**. Select the host pools you want to assign the scaling plan to and select **Assign**. The host pools must be in the same Azure region as the scaling plan.

## Next steps

- Review how to create a scaling plan at [Autoscale for Azure Virtual Desktop session hosts](autoscale-new-existing-host-pool.md).
- Learn how to troubleshoot your scaling plan at [Enable diagnostics for your scaling plan](autoscale-diagnostics.md).
- Learn more about terms used in this article at our [autoscale glossary](autoscale-glossary.md).
- For examples of how autoscale works, see [Autoscale example scenarios](autoscale-scenarios.md).
- View our [autoscale FAQ](autoscale-faq.yml) to answer commonly asked questions.

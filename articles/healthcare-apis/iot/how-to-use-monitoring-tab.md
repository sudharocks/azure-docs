---
title: How to use the MedTech service monitoring tab - Azure Health Data Services
description: This article describes how to use the MedTech service monitoring tab.
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 1/18/2023
ms.author: jasteppe
---

# How to use the MedTech service monitoring tab

In this article, you'll learn how to use the MedTech service monitoring tab in the Azure portal. The monitoring tab provides access to crucial MedTech service metrics. These metrics can be used in assessing the health and performance of your MedTech service and can be useful seeing patterns and/or trends or assisting with troubleshooting your MedTech service.

## Display the MedTech service monitoring tab metrics

1. Within your Azure Health Data Services workspace, select **MedTech service** under **Services**.

   :::image type="content" source="media\how-to-use-monitoring-tab\workspace-displayed-with-connectors-button.png" alt-text="Screenshot of select the MedTech service within the workspace." lightbox="media\how-to-use-monitoring-tab\workspace-displayed-with-connectors-button.png":::

2. Select the MedTech service that you would like to display metrics for. For this example, we'll select a MedTech service named **mt-azuredocsdemo**. You'll be selecting a MedTech service within your own Azure Health Data Services workspace.

   :::image type="content" source="media\how-to-use-monitoring-tab\select-medtech-service.png" alt-text="Screenshot of select the MedTech service you would like to display metrics for." lightbox="media\how-to-use-monitoring-tab\select-medtech-service.png":::

3. Select **Monitoring** tab within the MedTech service page.

   :::image type="content" source="media\how-to-use-monitoring-tab\select-monitoring-tab.png" alt-text="Screenshot of select the Metrics option within your MedTech service." lightbox="media\how-to-use-monitoring-tab\select-monitoring-tab.png":::

4. The MedTech service monitoring tab will open displaying a subset of the supported MedTech service metrics. By default, the **Show data for last** option is set to 1 hour. To adjust the time duration, select the **Show data for last option**, select the time period you would like to view, and select **Apply**. Select the down arrow in the **Traffic** MedTech service metrics tile to display the next set of MedTech service traffic metrics. 

   :::image type="content" source="media\how-to-use-monitoring-tab\display-metrics-tile.png" alt-text="Screenshot the MedTech service monitoring tab with drop-down menus." lightbox="media\how-to-use-monitoring-tab\display-metrics-tile.png":::

   > [!IMPORTANT]
   > If you leave the MedTech service monitoring tab, any customized settings you have made to the monitoring settings are lost and will have to be recreated. If you would like to save your customizations for future viewing, you can pin them to an Azure portal dashboard as a tile. 
   >
   > To learn how to customize and save metrics settings to an Azure portal dashboard and tile, see [How to configure the MedTech service metrics](how-to-configure-metrics.md).  

5. **Optional** - Select the **pin icon** to save the metrics tile to an Azure portal dashboard of your choosing.

   :::image type="content" source="media\how-to-use-monitoring-tab\pin-metrics-to-dashboard.png" alt-text="Screenshot the MedTech service monitoring tile with red box around the pin icon." lightbox="media\how-to-use-monitoring-tab\pin-metrics-to-dashboard.png":::
   
   > [!TIP]
   > To learn more about advanced metrics display and sharing options, see [Getting started with Azure Metrics Explorer](../../azure-monitor/essentials/metrics-getting-started.md)

## Available metrics for the MedTech service

This table shows the available MedTech service metrics and the information that the metrics are capturing. The metrics in **bold** are the metrics displayed within the **Monitoring** tab:  

Metric category|Metric name|Metric description|
|--------------|-----------|--------------|
|Availability|IotConnector Health Status|The overall health of the MedTech service.|
|Errors|**Total Error Count**|The total number of errors.|
|Latency|**Average Group Stage Latency**|The average latency of the group stage. The [group stage](understand-service.md#group) performs buffering, aggregating, and grouping on normalized messages.|
|Latency|**Average Normalize Stage Latency**|The average latency of the normalized stage. The [normalized stage](understand-service.md#normalize) performs normalization on raw incoming messages.|
|Traffic|Number of Fhir resources saved|The total number of Fast Healthcare Interoperability Resources (FHIR&#174;) resources [updated or persisted](understand-service.md#persist) by the MedTech service.|
|Traffic|**Number of Incoming Messages**|The number of received raw [incoming messages](understand-service.md#ingest) (for example, the device events) from the configured source event hub.|
|Traffic|**Number of Measurements**|The number of normalized value readings received by the FHIR [transformation stage](understand-service.md#transform) of the MedTech service.|
|Traffic|**Number of Message Groups**|The number of groups that have messages aggregated in the designated time window.|
|Traffic|**Number of Normalized Messages**|The number of normalized messages.|

## Next steps

In this article, you learned how to use the MedTech service monitoring tab.

To learn how to configure the MedTech service metrics, see

> [!div class="nextstepaction"]
> [How to configure the MedTech service metrics](how-to-configure-metrics.md)

To learn how to enable the MedTech service diagnostic settings, see

> [!div class="nextstepaction"]
> [How to enable diagnostic settings for the MedTech service](how-to-enable-diagnostic-settings.md)

(FHIR&#174;) is a registered trademark of Health Level Seven International, registered in the U.S. Trademark Office and is used with their permission.

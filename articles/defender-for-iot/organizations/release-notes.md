---
title: OT monitoring software versions - Microsoft Defender for IoT
description: This article lists Microsoft Defender for IoT on-premises OT monitoring software versions, including release and support dates and highlights for new features.
ms.topic: overview
ms.date: 1/02/2023
---

# OT monitoring software versions

The Microsoft Defender for IoT architecture uses on-premises sensors and management servers.

This article lists the supported software versions for the OT sensor and on-premises management software, including release dates, support dates, and highlights for the updated features.

For more information, including detailed descriptions and updates for cloud-only features, see [What's new in Microsoft Defender for IoT?](whats-new.md) Cloud-only features aren't dependent on specific sensor versions.

## Versioning and support for on-premises software versions

This section describes the servicing information, timelines, and guidance for the available on-premises software versions.

### Version update recommendations

When updating your on-premises software, we recommend:

- Plan to **update your sensor versions to the latest version once every 6 months**.

- Update to a **patch version only for specific bug fixes or security patches**. When working with the Microsoft support team on a specific issue, verify which patch version is recommended to resolve your issue.

> [!NOTE]
> If you have an on-premises management console, make sure to also update your on-premises management console to the same version as your sensors.
>

For more information, see [Update Defender for IoT OT monitoring software](update-ot-software.md).

### On-premises monitoring software versions

Cloud features may be dependent on a specific sensor version. Such features are listed below for the relevant software versions, and are only available for data coming from sensors that have the required version installed, or higher.

| Version / Patch |  Release date | Scope     | Supported until |
| ------- |  ------------ | ----------- | ------------------- |
| **22.3** | | | |
| 22.3.5 | 01/2023 | Patch | 12/2023 |
| 22.3.4 | 01/2023 | Major | 12/2023 |
| **22.2** | | | |
| 22.2.9 | 01/2023 | Patch | 12/2023 |
| 22.2.8 | 11/2022 | Patch | 10/2023 |
| 22.2.7| 10/2022   | Patch | 09/2023          |
| 22.2.6|09/2022 |Patch | 04/2023|
|22.2.5 |08/2022 | Patch| 04/2023 |
|22.2.4 |07/2022 |Patch |04/2023 |
| 22.2.3| 07/2022| Major| 04/2023|
| **22.1** | | | |
| 22.1.7| 07/2022 |Patch | 06/2023 |
| 22.1.6| 06/2022 |Patch |10/2022  |
| 22.1.5| 06/2022 |Patch | 10/2022 |
| 22.1.4|04/2022 | Patch|10/2022  |
| 22.1.3|03/2022 |Patch | 10/2022|
| 22.1.2| 02/2022 | Major|10/2022  |
| **10.5** | | | |
|10.5.5 |12/2021 |Patch |  09/2022|
|10.5.4 |12/2021 |Patch |  09/2022|
| 10.5.3| 10/2021 |Patch | 07/2022|
| 10.5.2| 10/2021 | Major| 07/2022|

### Threat intelligence updates

Threat intelligence updates are continuously available and are independent of specific sensor versions. You don't need to update your sensor version in order to get the latest threat intelligence updates.

For more information, see [Threat intelligence research and packages](how-to-work-with-threat-intelligence-packages.md).

### Support model

Defender for IoT provides **1 year of support** for every new version, starting with versions **22.1.7** and **22.2.7**. For example, version **22.2.7** was released in **October 2022** and is supported through **September 2023**.

Earlier versions use a legacy support model, with support dates [detailed for each version](#on-premises-monitoring-software-versions).

### On-premises appliance security

The OT network sensor and the on-premises management console are designed as a *locked-down* security appliance with a hardened attack surface. Appliance access and control is allowed only through the [management port](best-practices/understand-network-architecture.md), via HTTP for web access and SSH for the support shell.

Defender for IoT adheres to the [Microsoft Security Development Lifecycle](https://www.microsoft.com/securityengineering/sdl/) throughout the entire development lifecycle, including activities like training, compliance, code reviews, threat modeling, design requirements, component governance, and pen testing. All appliances are locked down according to industry best practices and should not be modified.

Maintain your sensors and on-premises management consoles, for activities like backups, log exports, or health monitoring, via the web interface, or the Defender for IoT [CLI commands](references-work-with-defender-for-iot-cli-commands.md).

> [!IMPORTANT]
> Manual changes to software packages or additions of external packages may have detrimental security or functional effects on the sensor and on-premises management console. Microsoft is unable to support deployments with manual changes made to software packages.
>

### Feature documentation per versions

Version numbers are listed only in this article and in the [What's new in Microsoft Defender for IoT?](whats-new.md) article, and not in detailed descriptions elsewhere in the documentation.

To understand whether a feature is supported in your sensor version, check the relevant version section below and its listed features.

## Versions 22.3.x

### 22.3.5

**Release date**: 01/2023

**Supported until**: 12/2023

This version includes bug fixes for stability improvements.

### 22.3.4

**Release date**: 01/2021

**Supported until**: 12/2023

- [Azure connectivity status shown on OT sensors](how-to-manage-individual-sensors.md#validate-connectivity-status)

## Versions 22.2.x

To update to 22.2.x versions:

- **From version 22.1.x**, update directly to the latest **22.2.x** version
- **From version 10.x**, first update to the latest **22.1.x** version, and then update again to the latest **22.2.x** version.

For more information, see [Update Defender for IoT OT monitoring software](update-ot-software.md).

### 22.2.9

**Release date**: 01/2023

**Supported until**: 12/2023

This version includes bug fixes for stability improvements.

### 22.2.8

**Release date**: 11/2022

**Supported until**: 10/2023

This version includes bug fixes for stability improvements.

### 22.2.7

**Release date**: 10/2022

**Supported until**: 09/2023

This version includes bug fixes for stability improvements.

### 22.2.6

**Release date**: 09/2022

**Supported until**: 04/2023

This version includes the following new updates and fixes:

- Bug fixes and stability improvements
- Enhancements to the device type classification algorithm

### 22.2.5

**Release date**: 08/2022

**Supported until**: 04/2023

This version includes minor stability improvements.

### 22.2.4

**Release date**: 07/2022

**Supported until**: 04/2023

This version includes the following new updates and fixes:

- [Device inventory enhancements in the sensor console](how-to-investigate-sensor-detections-in-a-device-inventory.md):

  - Merge duplicate devices, delete single devices, and delete inactive devices by admin users
  - **Last seen** value in the device details pane is replaced by **Last activity**

- [New parameters for the *devicecves* API](api/management-integration-apis.md): `sensorId`, `score`, and `deviceIds`

- [New alert columns with timestamp data](how-to-view-alerts.md): **Last detection**, **First detection**, and **Last activity**

### 22.2.3

**Release date**: 07/2022

**Supported until**: 04/2023

This version includes the following new updates and fixes:

- [Update your sensors from the Azure portal](update-ot-software.md#update-your-sensors)
- [New naming convention for hardware profiles](ot-appliance-sizing.md)
- [PCAP access from the Azure portal](how-to-manage-cloud-alerts.md)
- [Bi-directional alert synch between OT sensors and the Azure portal](alerts.md#managing-ot-alerts-in-a-hybrid-environment)
- [Sensor connections restored after certificate rotation](how-to-deploy-certificates.md)
- [Upload diagnostic logs for support tickets from the Azure portal](how-to-manage-sensors-on-the-cloud.md#upload-a-diagnostics-log-for-support)
- [Improved security for uploading protocol plugins](resources-manage-proprietary-protocols.md)
- [Sensor names shown in browser tabs](how-to-manage-individual-sensors.md)
- [Site-based access control on the Azure portal](manage-users-portal.md#manage-site-based-access-control-public-preview)

## Versions 22.1.x

Software versions 22.1.x support direct updates to the latest OT monitoring software versions available. For more information, see [Update Defender for IoT OT monitoring software](update-ot-software.md).

### 22.1.7

**Release date**: 07/2022

**Supported until**: 06/2023

This version includes the following new updates and fixes:

- [Identical passwords for *cyberx_host* and *cyberx* users created during installations and updates](how-to-install-software.md)

### 22.1.6

**Release date**: 06/2022

**Supported until**: 10/2022

This version minor maintenance updates for internal sensor components.

### 22.1.5

**Release date**: 06/2022

**Supported until**: 10/2022

This version minor updates to improve TI installation packages and software updates.

### 22.1.4

**Release date**: 04/2022

**Supported until**: 10/2022

This version includes the following new updates and fixes:

- [Extended device property data in the **Device inventory** page on the Azure portal](how-to-manage-device-inventory-for-organizations.md), for the **Description**, **Tags**. **Protocols**, **Scanner**, and **Last Activity** fields

### 22.1.3

**Release date**: 03/2022

**Supported until**: 10/2022

This version includes the following new updates and fixes:

- [Diagnostic logs automatically available to support for cloud-connected sensors](how-to-manage-individual-sensors.md#download-a-diagnostics-log-for-support)
- [Rockwell protocol: Device inventory shows PLC operating mode key state, run state, and security mode](how-to-manage-device-inventory-for-organizations.md)
- [Automatic CLI session timeouts](references-work-with-defender-for-iot-cli-commands.md)
- [Sensor health widgets in the Azure portal](how-to-manage-sensors-on-the-cloud.md#understand-sensor-health)

### 22.1.1

**Release date**: 02/2022

**Supported until**: 10/2022

This version includes the following new updates and fixes:

- [New sensor installation wizard](how-to-install-software.md)

- [Sensor redesign and unified Microsoft product experience](how-to-manage-individual-sensors.md)

- [Enhanced sensor Overview page](how-to-manage-individual-sensors.md)

- [New sensor diagnostics log](how-to-manage-individual-sensors.md#download-a-diagnostics-log-for-support)

- [Alert updates](how-to-view-alerts.md):

  - Contextual data for each alert
  - Refreshed alert statuses
  - Alert storage updates
  - A new **Backup Activity with Antivirus Signatures** alert
  - Alert management changes during software updates

- [Enhancements for creating custom alerts on the sensor](how-to-accelerate-alert-incident-response.md#create-custom-alert-rules-on-an-ot-sensor): Hit count data, advanced scheduling options, and more supported fields and protocols

- [Modified CLI commands](cli-ot-sensor.md): Including the following new commands:

  - `sudo dpkg-reconfigure iot-sensor`
  - `sudo dpkg-reconfigure iot-sensor`
  - `sudo dpkg-reconfigure iot-sensor`

- [Refreshed update process and update log](update-ot-software.md)

- [New connectivity models](architecture-connections.md)

- [New firewall requirements](how-to-set-up-your-network.md#sensor-access-to-azure-portal)

- [Improved support for Profinet DCP, Honeywell, and Windows endpoint detection protocols](concept-supported-protocols.md)

- [Sensor reports now accessible from the **Data Mining** page](how-to-create-data-mining-queries.md)

- [Updated process for sensor name changes](how-to-manage-individual-sensors.md#change-the-name-of-a-sensor)

- [Site-based access control on the Azure portal](manage-users-portal.md#manage-site-based-access-control-public-preview)

## Versions 10.5.x

To update your software to the latest version available, first update to version 22.1.7, and then update again to the latest 22.2.x version. For more information, see [Update Defender for IoT OT monitoring software](update-ot-software.md).

### 10.5.5

**Release date**: 12/2021

**Supported until**: 9/2022

This version minor maintenance updates.

### 10.5.4

**Release date**: 12/2021

**Supported until**: 09/2022

This version includes the following new updates and fixes:

- [New Microsoft Sentinel solution for Defender for IoT](../../sentinel/iot-solution.md)
- [Mitigation for the Apache Log4j vulnerability](https://techcommunity.microsoft.com/t5/microsoft-defender-for-iot/updated-15-dec-defender-for-iot-security-advisory-apache-log4j/m-p/3036844)
- [Alerts for minor events and edge cases disabled or minimized](alert-engine-messages.md)

### 10.5.3

**Release date**: 10/2021

**Supported until**: 07/2022

This version includes the following new updates and fixes:

- [New integration APIs](api/management-integration-apis.md)
- [Network traffic analysis enhancements for multiple OT and ICS protocols](concept-supported-protocols.md)
- [Automatic deletion for older, archived alerts](how-to-view-alerts.md)
- [Export alert enhancements](how-to-work-with-alerts-on-premises-management-console.md#export-alerts-to-a-csv-file)

### 10.5.2

**Release date**: 10/2021

**Supported until**: 07/2022

This version includes the following new updates and fixes:

- [PLC operating mode detections](how-to-create-risk-assessment-reports.md)
- [New PCAP API](api/management-alert-apis.md#pcap-request-alert-pcap)
- [Export logs from the on-premises management console for troubleshooting](how-to-troubleshoot-the-sensor-and-on-premises-management-console.md#export-logs-from-the-on-premises-management-console-for-troubleshooting)
- [Support for Webhook extended to send data to endpoints](how-to-forward-alert-information-to-partners.md#webhook-extended)
- [Unicode support for certificate passphrases](how-to-deploy-certificates.md)

## Next steps

For more information about the features listed in this article, see [What's new in Microsoft Defender for IoT?](whats-new.md) and [What's new archive for in Microsoft Defender for IoT for organizations](release-notes-archive.md).

---
title: Manage action groups in the Azure portal
description: Find out how to create and manage action groups. Learn about notifications and actions that action groups enable, such as email, webhooks, and Azure Functions.
author: jacegummersall
ms.topic: conceptual
ms.date: 09/07/2022
ms.author: jagummersall
ms.reviewer: jagummersall
ms.custom: references_regions

---
# Create and manage action groups in the Azure portal

When Azure Monitor data indicates that there might be a problem with your infrastructure or application, an alert is triggered. Azure Monitor, Azure Service Health, and Azure Advisor then use *action groups* to notify users about the alert and take an action. An action group is a collection of notification preferences that are defined by the owner of an Azure subscription.

This article shows you how to create and manage action groups in the Azure portal. Depending on your requirements, you can configure various alerts to use the same action group or different action groups.

Each action is made up of the following properties:

- **Type**: The notification that's sent or action that's performed. Examples include sending a voice call, SMS, or email. You can also trigger various types of automated actions. For detailed information about notification and action types, see [Action-specific information](#action-specific-information), later in this article.
- **Name**: A unique identifier within the action group.
- **Details**: The corresponding details that vary by type.

For information about how to use Azure Resource Manager templates to configure action groups, see [Action group Resource Manager templates](./action-groups-create-resource-manager-template.md).

An action group is a **global** service, so there's no dependency on a specific Azure region. Requests from clients can be processed by action group services in any region. For instance, if one region of the action group service is down, the traffic is automatically routed and processed by other regions. As a global service, an action group helps provide a **disaster recovery** solution.

## Create an action group by using the Azure portal

1. Go to the [Azure portal](https://portal.azure.com).

1. Search for and select **Monitor**. The **Monitor** pane consolidates all your monitoring settings and data in one view.

1. Select **Alerts**, and then select **Action groups**.

   :::image type="content" source="./media/action-groups/manage-action-groups.png" alt-text="Screenshot of the Alerts page in the Azure portal. The Action groups button is called out.":::

1. Select **Create**.

   :::image type="content" source="./media/action-groups/create-action-group.png" alt-text="Screenshot of the Action groups page in the Azure portal. The Create button is called out.":::

1. Enter information as explained in the following sections.

### Configure basic action group settings

1. Under **Project details**
   - Select values for **Subscription** and **Resource group**.
   - Select the region

      | Option | Behavior |
      | ------ | -------- |
      | Global | The action groups service decides where to store the action group. The action group is persisted in at least two regions to ensure regional resiliency. Processing of actions may be done in any [geographic region](https://azure.microsoft.com/explore/global-infrastructure/geographies/#overview).<br></br>Voice, SMS and email actions performed as the result of [service health alerts](../../service-health/alerts-activity-log-service-notifications-portal.md) are resilient to Azure live-site-incidents. |
      | Regional | The action group is stored within the selected region. The action group is [zone-redundant](../../availability-zones/az-region.md#highly-available-services). Processing of actions is performed within the region.</br></br>Use this option if you want to ensure that the processing of your action group is performed within a specific [geographic boundary](https://azure.microsoft.com/explore/global-infrastructure/geographies/#overview). |
   
   The action group is saved in the subscription, region and resource group that you select.

1. Under **Instance details**, enter values for **Action group name** and **Display name**. The display name is used in place of a full action group name when the group is used to send notifications.

   :::image type="content" source="./media/action-groups/action-group-1-basics.png" alt-text="Screenshot of the Create action group dialog box. Values are visible in the Subscription, Resource group, Action group name, and Display name boxes.":::

### Configure notifications

1. To open the **Notifications** tab, select **Next: Notifications**. Alternately, at the top of the page, select the **Notifications** tab.

1. Define a list of notifications to send when an alert is triggered. Provide the following information for each notification:

   - **Notification type**: Select the type of notification that you want to send. The available options are:

     - **Email Azure Resource Manager Role**: Send an email to users who are assigned to certain subscription-level Azure Resource Manager roles.
     - **Email/SMS message/Push/Voice**: Send various notification types to specific recipients.

   - **Name**: Enter a unique name for the notification.

   - **Details**: Based on the selected notification type, enter an email address, phone number, or other information.

   - **Common alert schema**: You can choose to turn on the common alert schema, which provides the advantage of having a single extensible and unified alert payload across all the alert services in Monitor. For more information about this schema, see [Common alert schema](./alerts-common-schema.md).

   :::image type="content" source="./media/action-groups/action-group-2-notifications.png" alt-text="Screenshot of the Notifications tab of the Create action group dialog box. Configuration information for an email notification is visible.":::

1. Select OK.

### Configure actions

1. To open the **Actions** tab, select **Next: Actions**. Alternately, at the top of the page, select the **Actions** tab.

1. Define a list of actions to trigger when an alert is triggered. Provide the following information for each action:

   - **Action type**: Select from the following types of actions:

     - An Azure Automation runbook
     - An Azure Functions function
     - A notification that's sent to Azure Event Hubs
     - A notification that's sent to an IT service management (ITSM) tool
     - An Azure Logic Apps workflow
     - A secure webhook
     - A webhook

   - **Name**: Enter a unique name for the action.

   - **Details**: Enter appropriate information for your selected action type. For instance, you might enter a webhook URI, the name of an Azure app, an ITSM connection, or an Automation runbook. For an ITSM action, also enter values for **Work item** and other fields that your ITSM tool requires.

   - **Common alert schema**: You can choose to turn on the common alert schema, which provides the advantage of having a single extensible and unified alert payload across all the alert services in Monitor. For more information about this schema, see [Common alert schema](./alerts-common-schema.md).

   :::image type="content" source="./media/action-groups/action-group-3-actions.png" alt-text="Screenshot of the Actions tab of the Create action group dialog box. Several options are visible in the Action type list.":::

### Create the action group

1. If you'd like to assign a key-value pair to the action group, select **Next: Tags** or the **Tags** tab. Otherwise, skip this step. By using tags, you can categorize your Azure resources. Tags are available for all Azure resources, resource groups, and subscriptions.

   :::image type="content" source="./media/action-groups/action-group-4-tags.png" alt-text="Screenshot of the Tags tab of the Create action group dialog box. Values are visible in the Name and Value boxes.":::

1. To review your settings, select **Review + create**. This step quickly checks your inputs to make sure you've entered all required information. If there are issues, they're reported here. After you've reviewed the settings, select **Create** to create the action group.

   :::image type="content" source="./media/action-groups/action-group-5-review.png" alt-text="Screenshot of the Review + create tab of the Create action group dialog box. All configured values are visible.":::

> [!NOTE]
>
> When you configure an action to notify a person by email or SMS, they receive a confirmation indicating that they have been added to the action group.

### Test an action group in the Azure portal (preview)

When you create or update an action group in the Azure portal, you can **test** the action group.

1. Define an action, as described in the previous few sections. Then select **Review + create**. 

> [!NOTE]
> 
> If you are editing an already exisitng action group, you must save changes to the action group before testing.

2. On the page that lists the information that you entered, select **Test action group**.

   :::image type="content" source="./media/action-groups/test-action-group.png" alt-text="Screenshot of test action group start page. A Test action group button is visible.":::

3. Select a sample type and the notification and action types that you want to test. Then select **Test**.

   :::image type="content" source="./media/action-groups/test-sample-action-group.png" alt-text="Screenshot of the Test sample action group page. An email notification type and a webhook action type are visible.":::

4. If you close the window or select **Back to test setup** while the test is running, the test is stopped, and you don't get test results.

   :::image type="content" source="./media/action-groups/stop-running-test.png" alt-text="Screenshot of the Test sample action group page. A dialog box contains a Stop button and asks the user about stopping the test.":::

5. When the test is complete, a test status of either **Success** or **Failed** appears. If the test failed and you'd like to get more information, select **View details**.

   :::image type="content" source="./media/action-groups/test-sample-failed.png" alt-text="Screenshot of the Test sample action group page. Error details are visible, and a white X on a red background indicates that a test failed.":::

You can use the information in the **Error details** section to understand the issue. Then you can edit, save changes, and test the action group again.

When you run a test and select a notification type, you get a message with "Test" in the subject. The tests provide a way to check that your action group works as expected before you enable it in a production environment. All the details and links in test email notifications are from a sample reference set.

#### Azure Resource Manager role membership requirements

The following table describes the role membership requirements that are needed for the *test actions* functionality:

| User's role membership | Existing action group | Existing resource group and new action group | New resource group and new action group |
| ---------- | ------------- | ----------- | ------------- |
| Subscription contributor | Supported | Supported | Supported |
| Resource group contributor | Supported | Supported | Not applicable |
| Action group resource contributor | Supported | Not applicable | Not applicable |
| Azure Monitor contributor | Supported | Supported | Not applicable |
| Custom role | Supported | Supported | Not applicable |

> [!NOTE]
>
> You can run a limited number of tests per time period. To check which limits apply to your situation, see [Rate limiting for voice, SMS, emails, Azure App push notifications, and webhook posts](./alerts-rate-limiting.md).
>
> When you configure an action group in the portal, you can opt in or out of the common alert schema.
>
> - To find common schema samples for all sample types, see [Common alert schema definitions for Test Action Group](./alerts-common-schema-test-action-definitions.md).
> - To find non-common schema alert definitions, see [Non-common alert schema definitions for Test Action Group](./alerts-non-common-schema-definitions.md).


## Create an action group with a Resource Manager template
You can use an [Azure Resource Manager template](../../azure-resource-manager/templates/syntax.md) to configure action groups. Using templates, you can automatically set up action groups that can be reused in certain types of alerts. These action groups ensure that all the correct parties are notified when an alert is triggered.

The basic steps are:

1. Create a template as a JSON file that describes how to create the action group.

2. Deploy the template by using [any deployment method](../../azure-resource-manager/templates/deploy-powershell.md).

### Resource Manager templates for an action group

To create an action group using a Resource Manager template, you create a resource of the type `Microsoft.Insights/actionGroups`. Then you fill in all related properties. Here are two sample templates that create an action group.

First template, describes how to create a Resource Manager template for an action group where the action definitions are hard-coded in the template. Second template, describes how to create a template that takes the webhook configuration information as input parameters when the template is deployed.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2021-09-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com",
            "useCommonAlertSchema": true

          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com",
            "useCommonAlertSchema": true
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1",
            "useCommonAlertSchema": true
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2",
            "useCommonAlertSchema": true
          }
        ],
         "SecurewebhookReceivers": [
          {
            "name": "contososecureHook",
            "serviceUri": "http://requestb.in/1bq63iu1",
            "useCommonAlertSchema": false
          },
          {
            "name": "contososecureHook2",
            "serviceUri": "http://requestb.in/1bq63iu2",
            "useCommonAlertSchema": false
          }
        ],
        "eventHubReceivers": [
          {
            "name": "contosoeventhub1",
            "subscriptionId": "replace with subscription id GUID",
            "eventHubNameSpace": "contosoeventHubNameSpace",
            "eventHubName": "contosoeventHub",
            "useCommonAlertSchema": true
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service Name."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2021-09-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]",
            "useCommonAlertSchema": true
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

## Manage your action groups

After you create an action group, you can view it in the portal:

1. From the **Monitor** page, select **Alerts**.
1. Select **Manage actions**. 
1. Select the action group that you want to manage. You can:

   - Add, edit, or remove actions.
   - Delete the action group.

## Action-specific information

The following sections provide information about the various actions and notifications that you can configure in an action group.

> [!NOTE]
>
> To check numeric limits on each type of action or notification, see [Subscription service limits for monitoring](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-monitor-limits).

### Automation runbook

To check limits on Automation runbook payloads, see [Automation limits](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits).

You may have a limited number of runbook actions per action group.

### Azure app push notifications

To enable push notifications to the Azure mobile app, provide the email address that you use as your account ID when you configure the Azure mobile app. For more information about the Azure mobile app, see [Get the Azure mobile app](https://azure.microsoft.com/features/azure-portal/mobile-app/).

You might have a limited number of Azure app actions per action group.

### Email

Ensure that your email filtering and any malware/spam prevention services are configured appropriately. Emails are sent from the following email addresses:
 
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

You may have a limited number of email actions per action group. For information about rate limits, see [Rate limiting for voice, SMS, emails, Azure App push notifications, and webhook posts](./alerts-rate-limiting.md).

### Email Azure Resource Manager role

When you use this type of notification, you can send email to the members of a subscription's role. Email is only sent to Azure Active Directory (Azure AD) **user** members of the role. Email isn't sent to Azure AD groups or service principals.

A notification email is sent only to the *primary email* address.

If your *primary email* doesn't receive notifications, take the following steps:

1. In the Azure portal, go to **Active Directory**.
1. On the left, select **All users**. On the right, a list of users appears.
1. Select the user whose *primary email* you'd like to review.

   :::image type="content" source="media/action-groups/active-directory-user-profile.png" alt-text="Screenshot of the Azure portal All users page. Information about one user is visible but is indecipherable." border="true":::

1. In the user profile, look under **Contact info** for an **Email** value. If it's blank:

   1. At the top of the page, select **Edit**.
   1. Enter an email address.
   1. At the top of the page, select **Save**.

   :::image type="content" source="media/action-groups/active-directory-add-primary-email.png" alt-text="Screenshot of a user profile page in the Azure portal. The Edit button and the Email box are called out." border="true":::

You may have a limited number of email actions per action group. To check which limits apply to your situation, see [Rate limiting for voice, SMS, emails, Azure App push notifications, and webhook posts](./alerts-rate-limiting.md).

When you set up the Azure Resource Manager role:

1. Assign an entity of type **"User"** to the role.
1. Make the assignment at the **subscription** level.
1. Make sure an email address is configured for the user in their **Azure AD profile**.

> [!NOTE]
>
> It can take up to **24 hours** for a customer to start receiving notifications after they add a new Azure Resource Manager role to their subscription.

### Event Hubs

An Event Hubs action publishes notifications to Event Hubs. For more information about Event Hubs, see [Azure Event Hubs—A big data streaming platform and event ingestion service](../../event-hubs/event-hubs-about.md). You can subscribe to the alert notification stream from your event receiver.

### Functions

An action that uses Functions calls an existing HTTP trigger endpoint in Functions. For more information about Functions, see [Azure Functions](../../azure-functions/functions-get-started.md). To handle a request, your endpoint must handle the HTTP POST verb.

When you define the function action, the function's HTTP trigger endpoint and access key are saved in the action definition, for example, `https://azfunctionurl.azurewebsites.net/api/httptrigger?code=<access_key>`. If you change the access key for the function, you need to remove and recreate the function action in the action group.

You may have a limited number of function actions per action group.

   > [!NOTE]
   >
   > The function must have access to the storage account. If not, no keys will be available and the function URI will not be accessible.
   > [Learn about restoring access to the storage account](../../azure-functions/functions-recover-storage-account.md)

### ITSM

An ITSM action requires an ITSM connection. To learn how to create an ITSM connection, see [ITSM integration](./itsmc-overview.md).

You might have a limited number of ITSM actions per action group.

### Logic Apps

You may have a limited number of Logic Apps actions per action group.

### Secure webhook

When you use a secure webhook action, you must use Azure AD to secure the connection between your action group and your protected web API, which is your webhook endpoint. 
 
The secure webhook Action authenticates to the protected API using a Service Principal instance in the AD tenant of the "AZNS AAD Webhook" Azure AD Application. To make the action group work, this Azure AD Webhook Service Principal needs to be added as member of a role on the target Azure AD application that grants access to the target endpoint.
 
For an overview of Azure AD applications and service principals, see [Microsoft identity platform (v2.0) overview](../../active-directory/develop/v2-overview.md). Follow these steps to take advantage of the secure webhook functionality.

> [!NOTE]
>
> Basic authentication is not supported for SecureWebhook. To use basic authentication you must use Webhook.

> [!NOTE]
>
> If you use the webhook action, your target webhook endpoint needs to be able to process the various JSON payloads that different alert sources emit. If the webhook endpoint expects a specific schema, for example, the Microsoft Teams schema, use the Logic Apps action to transform the alert schema to meet the target webhook's expectations.

1. Create an Azure AD application for your protected web API. For detailed information, see [Protected web API: App registration](../../active-directory/develop/scenario-protected-web-api-app-registration.md). Configure your protected API to be called by a daemon app, and expose application permissions, not delegated permissions. For more information about these permissions, see [If your web API is called by a service or daemon app](../../active-directory/develop/scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-service-or-daemon-app).

   > [!NOTE]
   >
   > Configure your protected web API to accept V2.0 access tokens. For detailed information about this setting, see [Azure Active Directory app manifest](../../active-directory/develop/reference-app-manifest.md#accesstokenacceptedversion-attribute).

1. To enable the action group to use your Azure AD application, use the PowerShell script that follows this procedure.

   > [!NOTE]
   >
   > You must be assigned the [Azure AD Application Administrator role](../../active-directory/roles/permissions-reference.md#all-roles) to run this script.

   1. Modify the PowerShell script's `Connect-AzureAD` call to use your Azure AD tenant ID.
   1. Modify the PowerShell script's `$myAzureADApplicationObjectId` variable to use the Object ID of your Azure AD application.
   1. Run the modified script.

   > [!NOTE]
   >
   > The service principle needs to be assigned an **owner role** of the Azure AD application to be able to create or modify the secure webhook action in the action group.

1. Configure the secure webhook action.

   1. Copy the `$myApp.ObjectId` value that's in the script.
   1. In the webhook action definition, in the **Object Id** box, enter the value that you copied.

   :::image type="content" source="./media/action-groups/action-groups-secure-webhook.png" alt-text="Screenshot of the Secured Webhook dialog box in the Azure portal. The Object ID box is visible." border="true":::

#### Secure webhook PowerShell script

```PowerShell
Connect-AzureAD -TenantId "<provide your Azure AD tenant ID here>"

# Define your Azure AD application's ObjectId.
$myAzureADApplicationObjectId = "<the Object ID of your Azure AD Application>"

# Define the action group Azure AD AppId.
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"

# Define the name of the new role that gets added to your Azure AD application.
$actionGroupRoleName = "ActionGroupsSecureWebhook"

# Create an application role with the given name and description.
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}

# Get your Azure AD application, its roles, and its service principal.
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles

# Create the role if it doesn't exist.
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "The Action Group role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")

    # Add the new role to the Azure AD application.
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Group to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}

# Create the service principal if it doesn't exist.
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the action group Azure AD application and add it to the role.
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}

New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId

Write-Host "My Azure AD Application (ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```

### SMS

For information about rate limits, see [Rate limiting for voice, SMS, emails, Azure App push notifications, and webhook posts](./alerts-rate-limiting.md).

For important information about using SMS notifications in action groups, see [SMS alert behavior in action groups](./alerts-sms-behavior.md).

You might have a limited number of SMS actions per action group.

> [!NOTE]
>
> If you can't select your country/region code in the Azure portal, SMS isn't supported for your country/region. If your country/region code isn't available, you can vote to have your country/region added at [Share your ideas](https://feedback.azure.com/d365community/idea/e527eaa6-2025-ec11-b6e6-000d3a4f09d0). In the meantime, as a workaround, configure your action group to call a webhook to a third-party SMS provider that offers support in your country/region.

For information about pricing for supported countries/regions, see [Azure Monitor pricing](https://azure.microsoft.com/pricing/details/monitor/).

#### Countries with SMS notification support

| Country code | Country |
|:---|:---|
| 61 | Australia |
| 43 | Austria |
| 32 | Belgium |
| 55 | Brazil |
| 1    |Canada |
| 56 | Chile |
| 86 | China |
| 420 | Czech Republic |
| 45 | Denmark |
| 372 | Estonia |
| 358 | Finland |
| 33 | France |
| 49 | Germany |
| 852 | Hong Kong |
| 91 | India |
| 353 | Ireland |
| 972 | Israel |
| 39 | Italy |
| 81 | Japan |
| 352 | Luxembourg |
| 60 | Malaysia |
| 52 | Mexico |
| 31 | Netherlands |
| 64 | New Zealand |
| 47 | Norway |
| 351 | Portugal |
| 1 | Puerto Rico |
| 40 | Romania |
| 7  | Russia  |
| 65 | Singapore |
| 27 | South Africa |
| 82 | South Korea |
| 34 | Spain |
| 41 | Switzerland |
| 886 | Taiwan |
| 971 | UAE    |
| 44 | United Kingdom |
| 1 | United States |

### Voice

For important information about rate limits, see [Rate limiting for voice, SMS, emails, Azure App push notifications, and webhook posts](./alerts-rate-limiting.md).

You might have a limited number of voice actions per action group.

> [!NOTE]
>
> If you can't select your country/region code in the Azure portal, voice calls aren't supported for your country/region. If your country/region code isn't available, you can vote to have your country/region added at [Share your ideas](https://feedback.azure.com/d365community/idea/e527eaa6-2025-ec11-b6e6-000d3a4f09d0). In the meantime, as a workaround, configure your action group to call a webhook to a third-party voice call provider that offers support in your country/region.
>
> The only country code that action groups currently support for voice notification is +1 for the United States.

For information about pricing for supported countries/regions, see [Azure Monitor pricing](https://azure.microsoft.com/pricing/details/monitor/).

### Webhook

> [!NOTE]
>
> If you use the webhook action, your target webhook endpoint needs to be able to process the various JSON payloads that different alert sources emit. If the webhook endpoint expects a specific schema, for example, the Microsoft Teams schema, use the Logic Apps action to transform the alert schema to meet the target webhook's expectations.

Webhook action groups use the following rules:

- A webhook call is attempted at most three times.

- The first call waits 10 seconds for a response.

- Between the first and second call it waits 20 seconds for a response.

- Between the second and third call it waits 40 seconds for a response.

- The call is retried if any of the following conditions are met:

  - A response isn't received within the timeout period.
  - One of the following HTTP status codes is returned: 408, 429, 503, 504 or TaskCancellationException.
  - If any one of the above errors is encountered an additional 5 seconds wait for the response.

- If three attempts to call the webhook fail, no action group calls the endpoint for 15 minutes.

For source IP address ranges, see [Action group IP addresses](../app/ip-addresses.md).

## Next steps

- Learn more about [SMS alert behavior](./alerts-sms-behavior.md).
- Gain an [understanding of the activity log alert webhook schema](./activity-log-alerts-webhook.md).
- Learn more about [ITSM Connector](./itsmc-overview.md).
- Learn more about [rate limiting](./alerts-rate-limiting.md) on alerts.
- Get an [overview of activity log alerts](./alerts-overview.md), and learn how to receive alerts.
- Learn how to [configure alerts whenever a Service Health notification is posted](../../service-health/alerts-activity-log-service-notifications-portal.md).

---
title: Lifecycle Workflows tasks and definitions
description: This article guides a user on Workflow task definitions and task parameters.
author: OWinfreyATL
ms.author: owinfrey
manager: amycolannino
ms.service: active-directory
ms.subservice: compliance
ms.workload: identity
ms.topic: conceptual
ms.date: 01/26/2023
---
# Lifecycle Workflow built-in tasks (preview)

Lifecycle Workflows come with many pre-configured tasks that are designed to automate common lifecycle management scenarios. These built-in tasks can be utilized to make customized workflows to suit your organization's needs. These tasks can be configured within seconds to create new workflows. These tasks also have categories based on the Joiner-Mover-Leaver model so that they can be easily placed into workflows based on need. In this article you'll get the complete list of tasks, information on common parameters each task has, and a list of unique parameters needed for each specific task.


## Supported tasks (preview)
 
Lifecycle Workflow's built-in tasks each include an identifier, known as **taskDefinitionID**, and can be used to create either new workflows from scratch, or inserted into workflow templates so that they fit the needs of your organization. For more information on templates available for use with Lifecycle Workflows, see: [Lifecycle Workflow Templates](lifecycle-workflow-templates.md).


[!INCLUDE [Lifecylce Workflows tasks table](../../../includes/lifecycle-workflows-tasks-table.md)]

## Common task parameters (preview)

Common task parameters are the non-unique parameters contained in every task. When adding tasks to a new workflow, or a workflow template, you can customize and configure these parameters so that they match your requirements.

|Parameter  |Definition  |
|---------|---------|
|category     |  A  read-only string that identifies the category or categories of the task. Automatically determined when the taskDefinitionID is chosen.     |
|taskDefinitionId     | A string referencing a taskDefinition that determines which task to run.       |
|isEnabled     | A boolean value that denotes whether the task is set to run or not. If set to “true" then the task will run. Defaults to true.       |
|displayName     |  A unique string that identifies the task.       |
|description     | A string that describes the purpose of the task for administrative use. (Optional)         |
|executionSequence     | A read-only integer that states in what order the task will run in a workflow. For more information about executionSequence and workflow order, see: [Configure Scope](understanding-lifecycle-workflows.md#configure-scope).       |
|continueOnError     |  A boolean value that determines if the failure of this task stops the subsequent workflows from running.        |
|arguments     |  Contains unique parameters relevant for the given task.       |



## Task details (preview)

Below is each specific task, and detailed information such as parameters and prerequisites, required for them to run successfully. The parameters are noted as they appear both in the Azure portal, and within Microsoft Graph. For information about editing Lifecycle Workflow tasks in general, see: [Manage workflow Versions](manage-workflow-tasks.md).


### Send welcome email to new hire


Lifecycle Workflows allow you to automate the sending of welcome emails to new hires in your organization. You're able to customize the task name and description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/welcome-email-task.png" alt-text="Screenshot of Workflows task: Welcome email task.":::


The Azure AD prerequisite to run the **Send welcome email to new hire** task is:

- A populated mail attribute for the user.


For Microsoft Graph the parameters for the **Send welcome email to new hire** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner      |
|displayName     | Send Welcome Email (Customizable by user)        |
|description     | Send welcome email to new hire (Customizable by user)      |
|taskDefinitionId     |   70b29d51-b59a-4773-9280-8841dfd3f2ea     |



```Example for usage within the workflow
{
            "category": "joiner",
            "continueOnError": true,
            "description": "Send welcome email to new hire",
            "displayName": "Send Welcome Email",
            "isEnabled": true,
            "taskDefinitionId": "70b29d51-b59a-4773-9280-8841dfd3f2ea",
            "arguments": []
}

```

### Send onboarding reminder email


Lifecycle Workflows allow you to automate the sending of onboarding reminder emails to managers of new hires in your organization. You're able to customize the task name and description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/send-onboarding-reminder-email.png" alt-text="Screenshot of Workflows task: Send onboarding reminder email task.":::


The Azure AD prerequisite to run the **Send onboarding reminder email** task is:

- A populated manager attribute for the user.
- A populated manager's mail attribute for the user.


For Microsoft Graph the parameters for the **Send onboarding reminder email** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner      |
|displayName     | Send onboarding reminder email (Customizable by user)        |
|description     | Send onboarding reminder email to user’s manager (Customizable by user)      |
|taskDefinitionId     |   3C860712-2D37-42A4-928F-5C93935D26A1     |



```Example for usage within the workflow
{
            "category": "joiner",
            "continueOnError": true,
            "description": "Send onboarding reminder email to user’s manager",
            "displayName": "Send onboarding reminder email",
            "isEnabled": true,
            "taskDefinitionId": "3C860712-2D37-42A4-928F-5C93935D26A1",
            "arguments": []
}

```

### Generate Temporary Access Pass and send via email to user's manager

When a compatible user joins your organization, Lifecycle Workflows allow you to automatically generate a Temporary Access Pass (TAP), and have it sent to the new user's manager.

> [!NOTE]
> The user's employee hire date is used as the start time for the Temporary Access Pass. Please make sure that the TAP lifetime task setting and the [time portion of your user's hire date](how-to-lifecycle-workflow-sync-attributes.md#importance-of-time) are set appropriately so that the TAP is still valid when the user starts their first day. If the hire date at the time of workflow execution is already in the past, the current time is used as the start time.

With this task in the Azure portal, you're able to give the task a name and description. You must also set:

- **Activation duration**- How long the passcode is active.
- **One time use**- If the passcode can only be used once.
:::image type="content" source="media/lifecycle-workflow-task/tap-task.png" alt-text="Screenshot of Workflows task: TAP task.":::
 

The Azure AD prerequisites to run the **Generate Temporary Access Pass and send via email to user's manager** task are:

- A populated manager attribute for the user.
- A populated manager's mail attribute for the user.
- The TAP tenant policy must be enabled and the selected values for activation duration and one time use must be within the allowed range of the policy. For more information, see [Enable the Temporary Access Pass policy](../authentication/howto-authentication-temporary-access-pass.md#enable-the-temporary-access-pass-policy)

> [!IMPORTANT]
> A user having this task run for them in a workflow must also not have any other authentication methods, sign-ins, or AAD role assignments for this task to work for them.

For Microsoft Graph the parameters for the **Generate Temporary Access Pass and send via email to user's manager** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner      |
|displayName     | GenerateTAPAndSendEmail (Customizable by user)      |
|description     | Generate Temporary Access Pass and send via email to user's manager (Customizable by user)       |
|taskDefinitionId     |   1b555e50-7f65-41d5-b514-5894a026d10d     |
|arguments     |  Argument contains the name parameter "tapLifetimeInMinutes", which is the lifetime of the temporaryAccessPass in minutes starting at startDateTime. Minimum 10, Maximum 43200 (equivalent to 30 days). The argument also contains the tapIsUsableOnce parameter, which determines whether the passcode is limited to a one time use. If true, the pass can be used once; if false, the pass can be used multiple times within the temporaryAccessPass lifetime.    |


```Example for usage within the workflow
{
    "category": "joiner",
    "description": "Generate Temporary Access Pass and send via email to user's manager",
    "displayName": "GenerateTAPAndSendEmail",
    "isEnabled": true,
    "continueOnError": true,
    "taskDefinitionId": "1b555e50-7f65-41d5-b514-5894a026d10d",
        "arguments": [
            {
                "name": "tapLifetimeMinutes",
                "value": "60"
            },
            {
                "name": "tapIsUsableOnce",
                "value": "true"
            }
        ]
}

```

### Add user to groups

Allows users to be added to Microsoft 365 and cloud-only security groups. Mail-enabled, distribution, dynamic and privileged access groups are not supported. To control access to on-premises applications and resources, you need to enable group writeback. For more information, see [Azure AD Connect group writeback](../hybrid/how-to-connect-group-writeback-v2.md). 

You're able to customize the task name and description for this task.
:::image type="content" source="media/lifecycle-workflow-task/add-group-task.png" alt-text="Screenshot of Workflows task: Add user to group task.":::


For Microsoft Graph the parameters for the **Add user to groups** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner, leaver      |
|displayName     |  AddUserToGroup (Customizable by user)        |
|description     |  Add user to groups (Customizable by user)       |
|taskDefinitionId     |   22085229-5809-45e8-97fd-270d28d66910      |
|arguments     |  Argument contains a name parameter that is the "groupID", and a value parameter that is the group ID of the group you're adding the user to.    |


```Example for usage within the workflow
{
            "category": "joiner,leaver",
            "description": "Add user to groups",
            "displayName": "AddUserToGroup",
            "isEnabled": true,
            "continueOnError": true,
            "taskDefinitionId": "22085229-5809-45e8-97fd-270d28d66910",
            "arguments": [
                {
                    "name": "groupID",
                    "value": "0732f92d-6eb5-4560-80a4-4bf242a7d501"
                }
            ]
}

```


### Add user to teams

You're able to add a user to an existing static team. You're able to customize the task name and description for this task.
:::image type="content" source="media/lifecycle-workflow-task/add-team-task.png" alt-text="Screenshot of Workflows task: add user to team.":::


For Microsoft Graph the parameters for the **Add user to teams** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner, leaver      |
|displayName     |  AddUserToTeam (Customizable by user)       |
|description     |  Add user to teams (Customizable by user)        |
|taskDefinitionId     |   e440ed8d-25a1-4618-84ce-091ed5be5594      |
|argument     |  Argument contains a name parameter that is the "teamID", and a value parameter that is the team ID of the existing team you're adding a user to.    |



```Example for usage within the workflow
{
            "category": "joiner,leaver",
            "description": "Add user to team",
            "displayName": "AddUserToTeam",
            "isEnabled": true,
            "continueOnError": true,
            "taskDefinitionId": "e440ed8d-25a1-4618-84ce-091ed5be5594",
            "arguments": [
                {
                    "name": "teamID",
                    "value": "e3cc382a-c4b6-4a8c-b26d-a9a3855421bd"
                }
            ]
}

```

### Enable user account

Allows cloud-only user accounts to be enabled. You're able to customize the task name and description for this task in the Azure portal.

:::image type="content" source="media/lifecycle-workflow-task/enable-task.png" alt-text="Screenshot of Workflows task: enable user account.":::


For Microsoft Graph the parameters for the **Enable user account** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner, leaver      |
|displayName     |  EnableUserAccount (Customizable by user)       |
|description     |  Enable user account (Customizable by user)        |
|taskDefinitionId     |   6fc52c9d-398b-4305-9763-15f42c1676fc      |



```Example for usage within the workflow
 {
            "category": "joiner,leaver",
            "description": "Enable user account",
            "displayName": "EnableUserAccount",
            "isEnabled": true,
            "continueOnError": true,
            "taskDefinitionId": "6fc52c9d-398b-4305-9763-15f42c1676fc",
            "arguments": []
}

```

### Run a Custom Task Extension

Workflows can be configured to launch a custom task extension. You're able to customize the task name and description for this task using the Azure portal.

:::image type="content" source="media/lifecycle-workflow-task/custom-extension-task.png" alt-text="Screenshot of Workflows task: custom extension task.":::

The Azure AD prerequisite to run the **Run a Custom Task Extension** task is:

- A Logic App that is compatible with the custom task extension. For more information, see: [Lifecycle workflow extensibility](lifecycle-workflow-extensibility.md).

For Microsoft Graph the parameters for the **Run a Custom Task Extension** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner, leaver      |
|displayName     | Run a Custom Task Extension (Customizable by user)        |
|description     |  Run a Custom Task Extension to call-out to an external system. (Customizable by user)      |
|taskDefinitionId     |   d79d1fcc-16be-490c-a865-f4533b1639ee      |
|argument     |  Argument contains a name parameter that is the "customTaskExtensionID", and a value parameter that is the ID of the previously created extension that contains information about the Logic App.    |




```Example for usage within the workflow
{
            "category": "joiner,leaver",
            "description": "Run a Custom Task Extension to call-out to an external system.",
            "displayName": "Run a Custom Task Extension",
            "isEnabled": true,
            "continueOnError": true,
            "taskDefinitionId": "d79d1fcc-16be-490c-a865-f4533b1639ee",
            "arguments": [
                {
                    "name": "customTaskExtensionID",
                    "value": "<ID of your Custom Task Extension>"
                }
            ]
}

```

For more information on setting up a Logic app to run with Lifecycle Workflows, see:[Trigger Logic Apps with custom Lifecycle Workflow tasks](trigger-custom-task.md).

### Disable user account

Allows cloud-only user accounts to be disabled. You're able to customize the task name and description for this task in the Azure portal.

:::image type="content" source="media/lifecycle-workflow-task/disable-task.png" alt-text="Screenshot of Workflows task: disable user account.":::


For Microsoft Graph the parameters for the **Disable user account** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner, leaver      |
|displayName     |  DisableUserAccount (Customizable by user)       |
|description     |  Disable user account (Customizable by user)       |
|taskDefinitionId     |   1dfdfcc7-52fa-4c2e-bf3a-e3919cc12950      |


```Example for usage within the workflow
{
            "category": "joiner,leaver",
            "description": "Disable user account",
            "displayName": "DisableUserAccount",
            "isEnabled": true,
            "continueOnError": true,
            "taskDefinitionId": "1dfdfcc7-52fa-4c2e-bf3a-e3919cc12950",
            "arguments": []
}

```

### Remove user from selected groups

Allows users to be removed from Microsoft 365 and cloud-only security groups. Mail-enabled, distribution, dynamic and privileged access groups are not supported. To control access to on-premises applications and resources, you need to enable group writeback. For more information, see [Azure AD Connect group writeback](../hybrid/how-to-connect-group-writeback-v2.md). 

You're able to customize the task name and description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/remove-group-task.png" alt-text="Screenshot of Workflows task: Remove user from select groups.":::



For Microsoft Graph the parameters for the **Remove user from selected groups** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  leaver      |
|displayName     |  Remove user from selected groups (Customizable by user)        |
|description     |  Remove user from membership of selected Azure AD groups (Customizable by user)      |
|taskDefinitionId     |   1953a66c-751c-45e5-8bfe-01462c70da3c      |
|argument     |  Argument contains a name parameter that is the "groupID", and a value parameter that is the group Id(s) of the group or groups you're removing the user from.   |



```Example for usage within the workflow
{
            "category": "leaver",
            "displayName": "Remove user from selected groups",
            "description": "Remove user from membership of selected Azure AD groups",
            "isEnabled": true,
            "continueOnError": true,
            "taskDefinitionId": "1953a66c-751c-45e5-8bfe-01462c70da3c",
            "arguments": [
                {
                    "name": "groupID",
                    "value": "GroupId1, GroupId2, GroupId3, ..."
                }
            ]
}

```

### Remove users from all groups

Allows users to be removed from every Microsoft 365 and cloud-only security group they're a member of. Mail-enabled, distribution, dynamic and privileged access groups are not supported. To control access to on-premises applications and resources, you need to enable group writeback. For more information, see [Azure AD Connect group writeback](../hybrid/how-to-connect-group-writeback-v2.md).


You're able to customize the task name and description for this task in the Azure portal.

 :::image type="content" source="media/lifecycle-workflow-task/remove-all-groups-task.png" alt-text="Screenshot of Workflows task: remove user from all groups.":::


For Microsoft Graph the parameters for the **Remove users from all groups** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  leaver      |
|displayName     |  Remove user from all groups (Customizable by user)       |
|description     |  Remove user from all Azure AD groups memberships (Customizable by user)        |
|taskDefinitionId     |   b3a31406-2a15-4c9a-b25b-a658fa5f07fc      |



```Example for usage within the workflow
{
            "category": "leaver",
            "continueOnError": true,
            "displayName": "Remove user from all groups",
            "description": "Remove user from all Azure AD groups memberships",
            "isEnabled": true,
            "taskDefinitionId": "b3a31406-2a15-4c9a-b25b-a658fa5f07fc",
            "arguments": []
}

```

### Remove User from Teams

Allows a user to be removed from one or multiple static teams. You're able to customize the task name and description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/remove-user-team-task.png" alt-text="Screenshot of Workflows task: remove user from teams.":::

For Microsoft Graph the parameters for the **Remove User from Teams** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  joiner, leaver      |
|displayName     |  Remove user from selected Teams (Customizable by user)       |
|description     |  Remove user from membership of selected Teams (Customizable by user)       |
|taskDefinitionId     |   06aa7acb-01af-4824-8899-b14e5ed788d6     |
|arguments     |  Argument contains a name parameter that is the "teamID", and a value parameter that is the Teams ID of the Teams you're removing the user from.   |


```Example for usage within the workflow
{
            "category": "joiner,leaver",
            "continueOnError": true,
            "displayName": "Remove user from selected Teams",
            "description": "Remove user from membership of selected Teams",
            "isEnabled": true,
            "taskDefinitionId": "06aa7acb-01af-4824-8899-b14e5ed788d6",
            "arguments": [
                {
                    "name": "teamID",
                    "value": "TeamId1, TeamId2, TeamId3, ..."
                }
            ]
}

```

### Remove users from all teams

Allows users to be removed from every static team they're a member of. You're able to customize the task name and description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/remove-user-all-team-task.png" alt-text="Screenshot of Workflows task: remove user from all teams.":::

For Microsoft Graph the parameters for the **Remove users from all teams** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  leaver      |
|displayName     |  Remove user from all Teams memberships (Customizable by user)       |
|description     |  Remove user from all Teams (Customizable by user)        |
|taskDefinitionId     |   81f7b200-2816-4b3b-8c5d-dc556f07b024      |



```Example for usage within the workflow
{
            "category": "leaver",
            "continueOnError": true,
            "description": "Remove user from all Teams",
            "displayName": "Remove user from all Teams memberships",
            "isEnabled": true,
            "taskDefinitionId": "81f7b200-2816-4b3b-8c5d-dc556f07b024",
            "arguments": []
}

```

### Remove all license assignments from User

Allows all direct license assignments to be removed from a user. For group-based license assignments, you would run a task to remove the user from the group the license assignment is part of.

You're able to customize the task name and description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/remove-license-assignment-task.png" alt-text="Screenshot of Workflows task: remove all licenses from users.":::

For Microsoft Graph the parameters for the **Remove all license assignment from user** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  leaver      |
|displayName     |  Remove all licenses for user (Customizable by user)        |
|description     |  Remove all licenses assigned to the user (Customizable by user)        |
|taskDefinitionId     |   8fa97d28-3e52-4985-b3a9-a1126f9b8b4e      |



```Example for usage within the workflow
{
            "category": "leaver",
            "continueOnError": true,
            "displayName": "Remove all licenses for user",
            "description": "Remove all licenses assigned to the user",
            "isEnabled": true,
            "taskDefinitionId": "8fa97d28-3e52-4985-b3a9-a1126f9b8b4e",
            "arguments": []
}

```

### Delete User

Allows cloud-only user accounts to be deleted. You're able to customize the task name and description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/delete-user-task.png" alt-text="Screenshot of Workflows task: Delete user account.":::


For Microsoft Graph the parameters for the **Delete User** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  leaver      |
|displayName     |  Delete user account (Customizable by user)      |
|description     |  Delete user account in Azure AD (Customizable by user)      |
|taskDefinitionId     |   8d18588d-9ad3-4c0f-99d0-ec215f0e3dff      |



```Example for usage within the workflow
{
            "category": "leaver",
            "continueOnError": true,
            "displayName": "Delete user account",
            "description": "Delete user account in Azure AD",
            "isEnabled": true,
            "taskDefinitionId": "8d18588d-9ad3-4c0f-99d0-ec215f0e3dff",
            "arguments": []
}

```

## Send email to manager before user last day

Allows an email to be sent to a user's manager before their last day. You're able to customize the task name and the description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/send-email-before-last-day.png" alt-text="Screenshot of Workflows task: send email before user last day task.":::


The Azure AD prerequisite to run the **Send email before user last day** task are:

- A populated manager attribute for the user.
- A populated manager's mail attribute for the user.

For Microsoft Graph the parameters for the **Send email before user last day** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  leaver      |
|displayName     |  Send email before user’s last day (Customizable by user)       |
|description     |  Send offboarding email to user’s manager before the last day of work (Customizable by user)        |
|taskDefinitionId     |   52853a3e-f4e5-4eb8-bb24-1ac09a1da935      |

```Example for usage within the workflow
{
            "category": "leaver",
            "continueOnError": true,
            "displayName": "Send email before user’s last day",
            "description": "Send offboarding email to user’s manager before the last day of work",
            "isEnabled": true,
            "taskDefinitionId": "52853a3e-f4e5-4eb8-bb24-1ac09a1da935",
            "arguments": []
}

```

## Send email on users last day

Allows an email to be sent to a user's manager on their last day. You're able to customize the task name and the description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/send-email-last-day.png" alt-text="Screenshot of Workflows task: task to send email last day.":::

The Azure AD prerequisite to run the **Send email on user last day** task are:

- A populated manager attribute for the user.
- A populated manager's mail attribute for the user.

For Microsoft Graph the parameters for the **Send email on user last day** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  leaver      |
|displayName     |  Send email on user’s last day (Customizable by user)       |
|description     |  Send offboarding email to user’s manager on the last day of work (Customizable by user)        |
|taskDefinitionId     |   9c0a1eaf-5bda-4392-9d9e-6e155bb57411      |

```Example for usage within the workflow
{
            "category": "leaver",
            "continueOnError": true,
            "displayName": "Send email on user’s last day",
            "description": "Send offboarding email to user’s manager on the last day of work",
            "isEnabled": true,
            "taskDefinitionId": "9c0a1eaf-5bda-4392-9d9e-6e155bb57411",
            "arguments": []
}

```

## Send offboarding email to users manager after their last day

Allows an email containing offboarding information to be sent to the user's manager after their last day. You're able to customize the task name and description for this task in the Azure portal.
:::image type="content" source="media/lifecycle-workflow-task/offboard-email-manager.png" alt-text="Screenshot of Workflows task: send offboarding email to users manager after their last day.":::

The Azure AD prerequisite to run the **Send offboarding email to users manager after their last day** task are:

- A populated manager attribute for the user.
- A populated manager's mail attribute for the user.


For Microsoft Graph the parameters for the **Send offboarding email to users manager after their last day** task are as follows:

|Parameter |Definition  |
|---------|---------|
|category    |  leaver      |
|displayName     |  Send offboarding email to user’s manager after the last day of work (Customizable by user)       |
|description     |  Remove user from all Teams (Customizable by user)        |
|taskDefinitionId     |   6f22ddd4-b3a5-47a4-a846-0d7c201a49ce      |

```Example for usage within the workflow
{
            "category": "leaver",
            "continueOnError": true,
            "displayName": "Send offboarding email to user’s manager after the last day of work",
            "description": "Send email after user’s last day",
            "isEnabled": true,
            "taskDefinitionId": "6f22ddd4-b3a5-47a4-a846-0d7c201a49ce",
            "arguments": []
}

```

## Next steps

- [Manage lifecycle workflows properties](manage-workflow-properties.md)
- [Manage lifecycle workflow versions](manage-workflow-tasks.md)


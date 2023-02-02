## Common components

The diagram shows the components of Dev Box and Deployments Environments. Dev centers and projects are common to both
services. Both use managed identities to support authentication, and both services provide access for developers through the developer portal.

:::image type="content" source="media/common-elements.png" alt-text="Diagram showing components of Microsoft Dev Box and Azure Deployment Environments.":::

The table lists Dev Box and Deployments Environments components.

|  Component             |   Dev Box    | Deployment Environments |
|------------------------|--------------|-------------------------|
|  Dev centers           |     Yes      |         Yes             |
|  Projects              |     Yes      |         Yes             |
|  Dev box pools         |     Yes      |         No              |
|  Dev box definitions   |     Yes      |         No              |
|  Dev boxes             |     Yes      |         No              |
|  Compute Galleries     |     Yes      |         No              |
|  Managed Identities    |     Yes      |         Yes             |
|  Environment types     |     No       |         Yes             |
|  Environments          |     No       |         Yes             |
|  Catalogs              |     No       |         Yes             |
|  Developer Portal      |     Yes      |         Yes             |


## How components appear in the Azure portal

As you work with Dev Box or Deployments Environments in the Azure portal, you can access components from both services.

In the dev center overview, you can access options for:

1. Configuring your dev boxes

2. Configuring your environments

:::image type="content" source="media/dev-center-overview.png" alt-text="Screenshot showing Dev Box and Deployment Environments components in a dev center.":::

In the projects overview, you can access options for:

1. Configuring environment types for a specific project

2. Managing dev box pools and environments

:::image type="content" source="media/project-overview.png" alt-text="Screenshot showing Dev Box and Deployment Environments components in a project.":::

You may also see informational messages that refer to a service you
aren't using, like this one from the projects overview:

:::image type="content" source="media/project-informational.png" alt-text="Screenshot showing an informational message.":::

If you're not configuring the service named in the informational
message, you can safely ignore it.

## Developer Portal

You can access your dev boxes and deployment environments through the
[developer portal](https://devportal.microsoft.com/). You'll see
existing dev boxes and environments displayed as shown in this
screenshot:

:::image type="content" source="media/developer-portal.png" alt-text="Screenshot showing dev boxes and environments in the developer portal.":::

You can create new dev boxes and environments through the developer
portal menu at the top right.

:::image type="content" source="media/developer-portal-menu.png" alt-text="Screenshot of developer portal menu.":::

And you can manage your existing dev boxes and environments too,
selecting the more actions menu on the relevant card. Here's an example
of managing an environment.

:::image type="content" source="media/manage-environment.png" alt-text="Screenshot of options for managing an environment.":::

## Next steps

- To learn how to configure Microsoft Dev Box, go to: [Quickstart: Configure the Microsoft Dev Box Preview service](../../articles/dev-box/quickstart-configure-dev-box-service.md).

- To learn how to configure Azure Deployment Environments, go to: [Quickstart: Create and configure a dev center](../../articles/deployment-environments/quickstart-create-and-configure-devcenter.md).
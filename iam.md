---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Managing access
{: #iam}

Access to {{site.data.keyword.satellitelong}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).
{: shortdesc}

The name for the {{site.data.keyword.satellitelong_notm}} service in IAM is
- **{{site.data.keyword.satellitelong_notm}}** in the UI
- **satellite** in the API and CLI

## Understanding {{site.data.keyword.satelliteshort}} resource types in IAM
{: #iam-resource-types}

You can use {{site.data.keyword.cloud_notm}} IAM to assign access to different resources in {{site.data.keyword.satelliteshort}}. The way that access works varies depending on the resource that you want to authorize.
{: shortdesc}

- [{{site.data.keyword.satelliteshort}} location](#iam-resource-loc), including actions for locations and hosts.
- [{{site.data.keyword.satelliteshort}} Config](#iam-resource-config), including actions for configurations, subscriptions, clusters, cluster groups, resources, and other components that use {{site.data.keyword.satelliteshort}} Config like storage.
- [{{site.data.keyword.satelliteshort}} Link](#iam-resource-link), including actions for endpoints and sources.
- [Other services](#iam-resource-services), like {{site.data.keyword.openshiftlong_notm}} clusters and {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services.

### Location
{: #iam-resource-loc}

Review details about the {{site.data.keyword.satelliteshort}} location IAM resource type.
{: shortdesc}

If you scope an access policy to the `location` resource type, the users must target the regional endpoint to interact with the location. For more information, see the [troubleshooting topic](/docs/satellite?topic=satellite-ts-location-missing-location).
{: note}

Name of the resource type
:    UI: `Location`
:    API or CLI: `location`

Type of role that you can assign for the resource in IAM
:    Platform access **Viewer**, **Operator**, **Editor**, and **Administrator** roles
:    Custom service access role to create clusters, **{{site.data.keyword.satelliteshort}} Cluster Creator**

What you can scope an access policy for the resource to
:    Account
:    Resource group
:    Particular instances of the resource

Description
:    [Locations](/docs/satellite?topic=satellite-locations) are places that you use to extend {{site.data.keyword.cloud_notm}} by attaching your own host compute machines to the location. Access to the location resource lets users work with locations and hosts. However, location access does not grant access to other resources that run within the location, such as endpoints, configurations, or {{site.data.keyword.redhat_openshift_notm}} clusters.

### Configuration, subscription, cluster, cluster group, and resource
{: #iam-resource-config}

Review details about the {{site.data.keyword.satelliteshort}} Config IAM resource type.
{: shortdesc}

Name of the resource type
:    UI: `Configuration`, `Subscription`, `Cluster`, `Clustergroup`, or `Resource`
:    API or CLI: `configuration`, `subscription`, `cluster`, `clustergroup`, or `resource`

Type of role that you can assign for the resource in IAM
:    Platform access **Viewer**, **Operator**, **Editor**, and **Administrator** roles
:    Service access **Reader**, **Writer**, and **Manager** roles, and a custom **Deployer** role

What you can scope an access policy for the resource to
:    Account
:    **Cluster** or **Clustergroup** only: Particular instance of the resource

Description
:    [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig) is a collection of configurations, versions, and subscriptions that you use to automatically deploy Kubernetes resources to groups of clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component. However, access to {{site.data.keyword.satelliteshort}} Config does not give a user access to the clusters that run the Kubernetes resources of the configuration. You can scope access to the following {{site.data.keyword.satelliteshort}} Config resources.
     - **Configurations**, where you upload the version of the configuration file for the Kubernetes resources that you want to deploy. You cannot scope a policy to a particular configuration.
     - **Subscriptions**, which you use to use to specify the cluster group where you want to deploy the Kubernetes resource definition that you added as a version to your configuration. You cannot scope a policy to a particular configuration.
     - **Clusters** or **cluster groups**, which are {{site.data.keyword.openshiftlong_notm}} that are registered with {{site.data.keyword.satelliteshort}} Config and can be subscribed to configurations.
     - **Resources**, which are Kubernetes resources such as pods or services that are described in a {{site.data.keyword.satelliteshort}} Config and run in a subscribed cluster. Certain roles permit access to view and manage Kubernetes resource through {{site.data.keyword.satelliteshort}} Config, but you cannot scope an access policy to a particular resource.

### Link
{: #iam-resource-link}

Review details about the {{site.data.keyword.satelliteshort}} Link IAM resource type.
{: shortdesc}

Name of the resource type
:    UI: `Link`
:    API or CLI: `link`

Type of role that you can assign for the resource in IAM
:    Platform access **Viewer**, **Operator**, **Editor**, and **Administrator** roles
:    Custom **{{site.data.keyword.satelliteshort}} Link Administrator** and **{{site.data.keyword.satelliteshort}} Link Source Access Controller** service access roles

What you can scope an access policy for the resource to
:    Account
:    Resource group
:    Particular instances of the resource

Description
:    [Link endpoints](/docs/satellite?topic=satellite-link-location-cloud) connect services, servers, or apps that run in your {{site.data.keyword.satelliteshort}} location with an endpoint that runs in {{site.data.keyword.cloud_notm}}. Access to a {{site.data.keyword.satelliteshort}} Link does not give a user access to the resources that the endpoint connects, such as a location or service instance. Instead, the access is to manage the endpoint itself.

### Other services
{: #iam-resource-services}

Review details about other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service IAM resource types.
{: shortdesc}

Resource type, IAM role, and scope of access policies
:    Varies by service. For example, {{site.data.keyword.openshiftlong_notm}} is the **Kubernetes Service** in IAM and can scope access to cluster or namespace resources. For more information, consult the service documentation.

Description for {{site.data.keyword.openshiftlong_notm}} clusters
:    You do not assign access policies for {{site.data.keyword.redhat_openshift_notm}} clusters in {{site.data.keyword.satelliteshort}}. Instead, access to clusters is assigned in {{site.data.keyword.cloud_notm}} IAM through {{site.data.keyword.openshiftlong_notm}} (**Kubernetes Service** in the console or `containers-kubernetes` in the API or CLI). For more information, see [Platform and service roles for {{site.data.keyword.redhat_openshift_notm}} clusters](#iam-roles-clusters).

     If you have access to a {{site.data.keyword.satelliteshort}} location or configuration, you can view the clusters that are attached to the location or configuration. However, you might not be able to access the clusters if you do not have the appropriate roles to those clusters. For example, if you have the appropriate access to a {{site.data.keyword.satelliteshort}} configuration, you might be able to list all the Kubernetes resources that run in registered clusters via the {{site.data.keyword.satelliteshort}} Config API. However, without an access policy to the individual clusters, you cannot log in to the individual clusters and use {{site.data.keyword.redhat_openshift_notm}} APIs to list Kubernetes resources.

Description of other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services
:    Similar to {{site.data.keyword.redhat_openshift_notm}} clusters, authorization to other {{site.data.keyword.cloud_notm}} services that are enabled in {{site.data.keyword.satelliteshort}} is managed in {{site.data.keyword.cloud_notm}} IAM through those services. For more information, see each service's documentation.

## Assigning access with {{site.data.keyword.cloud_notm}} IAM
{: #iam-assign}

To grant access to {{site.data.keyword.satelliteshort}} resources, use {{site.data.keyword.cloud_notm}} IAM. For information about assigning user roles in the console, see [Managing access to resources](/docs/account?topic=account-assign-access-resources).
{: shortdesc}

### Overview of the process to set up access to {{site.data.keyword.satellitelong_notm}} in {{site.data.keyword.cloud_notm}} IAM
{: #iam-assign-overview}

As a general practice, you can invite users to your {{site.data.keyword.cloud_notm}} account, add them to an access group, and assign them access to {{site.data.keyword.satellitelong_notm}} resources in IAM. You might also add access policies for other {{site.data.keyword.cloud_notm}} services, or assign individual user access.
{: shortdesc}

1. [Invite users to your account](/docs/account?topic=account-iamuserinv).
2. [Create an access group](/docs/account?topic=account-groups#create_ag) to add users to.
3. [Assign the access group](/docs/account?topic=account-groups#access_ag) with the appropriate scope for the {{site.data.keyword.satelliteshort}} resources and IAM platform and service roles for the actions you want to let users in your access group perform.
    - To scope access to the service, use **{{site.data.keyword.satellitelong_notm}}** in the UI or **satellite** in the API or CLI.
    - You can scope access to the account or particular resource groups. Keep in mind the following points.
        - Account-level access is not the same as access to all resource groups.
        - Not all {{site.data.keyword.satelliteshort}} resource types support scoping to resource groups. For example, you cannot scope {{site.data.keyword.satelliteshort}} Config resource types (configuration, subscription, cluster, or cluster group) to resource groups, only to the account.
    - For help with scoping the role to the right {{site.data.keyword.satelliteshort}} resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](#iam-resource-types). You can scope access policies to the following resource types.
        - Configuration
        - Cluster
        - Cluster group
        - Link
        - Location (when scoped, users must [target the regional endpoint](/docs/satellite?topic=satellite-ts-location-missing-location))
        - Subscription
    - You can further scope access to a particular resource for the following resource types.
        - Cluster
        - Cluster group
        - Link
        - Location (when scoped, users must [target the regional endpoint](/docs/satellite?topic=satellite-ts-location-missing-location))
    - For help with choosing the right platform and service roles, see the following reference information:
        - [Platform access roles](#iam-roles-platform)
        - [Service access roles](#iam-roles-service)
        - [Common use cases and roles](#iam-roles-usecases)
    - Consider creating a **Reader** service policy to {{site.data.keyword.satellitelong_notm}} (and not scoped to a particular resource type or resource) so that users can view the {{site.data.keyword.satelliteshort}} Config resources that run in {{site.data.keyword.satelliteshort}} clusters, such as pods or deployments.
4. [Assign the access group](/docs/account?topic=account-groups#access_ag) with the appropriate scope for any other {{site.data.keyword.cloud_notm}} services that you plan to use in your {{site.data.keyword.satelliteshort}} location. Refer to each service documentation for the level of access that you need. Common services include:
    - {{site.data.keyword.openshiftlong_notm}} clusters: **Kubernetes Service** in the UI, **containers-kubernetes** in the API and CLI.
    - {{site.data.keyword.registrylong_notm}} for a private registry across clusters: **Container Registry** in the UI, **container-registry** in the API and CLI.
    - {{site.data.keyword.cos_full_notm}} for the backing storage for your location information: **Cloud Object Storage** in the UI, **cos** in the API and CLI.
5. [Assign the access group](/docs/account?topic=account-groups#access_ag) with the **Viewer** platform access role to any resource groups that you plan to use with {{site.data.keyword.satelliteshort}}.


### Assigning access policy to access group by using the console
{: #iam-assign-ui}

Use the {{site.data.keyword.cloud_notm}} IAM console to assign an access policy to an access group to manage {{site.data.keyword.satelliteshort}} locations, hosts, and endpoints as shown in the following example.
{: shortdesc}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. From the menu bar, click **Manage > Access (IAM)**.
3. Click **Access groups**, and then click the access group that you want to assign access to {{site.data.keyword.satellitelong_notm}}.
4. Click the **Access policies** tab, and then click **Assign access**.
5. With the **IAM Services** tile selected, in the service access dropdown field, select **{{site.data.keyword.satellitelong_notm}}**.

    You can start to enter letters like `sat` and the field filters results to help you find **{{site.data.keyword.satellitelong_notm}}**.
    {: tip}

6. Leave the setting in **Account** so that you can scope the resource to a specific instance.
7. For **Resource Type** string equals field, scope the policy to a {{site.data.keyword.satelliteshort}} resource, such as **Location**.
8. For the **Resource** string equals field, enter the name of your {{site.data.keyword.satelliteshort}} location, such as **Port-NewYork**. Keep in mind the following considerations for various {{site.data.keyword.satelliteshort}} resources.
    - **{{site.data.keyword.satelliteshort}} location**: If you leave the **Resource** field blank, the user gets access to all the locations, which is needed for the user to create a location. When scoped to a location, users must [target the regional endpoint](/docs/satellite?topic=satellite-ts-location-missing-location).
    - **{{site.data.keyword.satelliteshort}} Config**: You cannot scope a policy to individual `configuration` or `subscription` resources. Instead, leave the **Resource** field blank and control access to your {{site.data.keyword.satelliteshort}} Config resources at the `clustergroup` level.
9. For **Platform access**, select the **Editor** role so that all users in your access group can add and remove hosts and endpoints from the {{site.data.keyword.satelliteshort}} location, but cannot create or delete locations. For other roles by resource type, see [IAM platform and service roles](#iam-roles).
10. Click **Add+**.
11. In the **Access summary** pane, review the access policy, and then click **Assign**.
12. From the access group **Access policies** table, verify that the Editor policy is added to the access group.


### Assigning access policy to access group with the CLI
{: #iam-assign-cli}

Use the {{site.data.keyword.cloud_notm}} IAM CLI to grant an access policy to an access group to manage {{site.data.keyword.satelliteshort}} resources as shown in the following example.
{: shortdesc}

1. Log in to {{site.data.keyword.cloud_notm}}. If you have a federated account, include the `--sso` option.

    ```sh
    ibmcloud login [--sso]
    ```
    {: pre}

2. Create an {{site.data.keyword.cloud_notm}} IAM access policy for {{site.data.keyword.satellitelong_notm}}. Scope the access policy based on what you want to assign access to. For more information, review the following example commands and table.

    For example, run the following command to assign a user the Administrator role for all your {{site.data.keyword.satelliteshort}} locations in the default resource group.
    
    ```sh
    ibmcloud iam user-policy-create user@email.com --service-name satellite --resource-group-name default --resource-type location --roles Administrator
    ```
    {: pre}

    Run the following command to assign an access group the Editor role to a specific {{site.data.keyword.satelliteshort}} location.
    
    ```sh
    ibmcloud iam access-group-policy-create team1 --service-name satellite --resource-type location --resource Port-NewYork --roles Editor
    ```
    {: pre}
    
    | Scope | Description |
    | ------------ | -------------- |
    | User  \n CLI option: N/A | You can assign the policy to an individual or group of users. Place this positional argument immediately following the command. For an individual user, enter the email address of the user. For an access group, enter the name of the access group of users. You can create an access group with the `ibmcloud iam access-group-create` command. To list available access groups, run `ibmcloud iam access-groups`. To add a user to an access group, run `ibmcloud iam access-group-user-add <access_group_name> <user_email>`. | 
    | {{site.data.keyword.cloud_notm}} service  \n CLI option: `--service-name` | Enter `satellite` to scope the access policy to {{site.data.keyword.satellitelong_notm}}. |
    | Resource group  \n CLI option: `--resource-group-name` | You can grant a policy for a resource group. If you do not specify a resource group, the policy applies to all service instances for all resource groups. To list available resource groups, run `ibmcloud resource groups`. |
    | {{site.data.keyword.satelliteshort}} resource   \n CLI option: `--resource-type` | You can limit the policy to a type of resource within {{site.data.keyword.satellitelong_notm}}, such as all {{site.data.keyword.satelliteshort}} locations or {{site.data.keyword.satelliteshort}} configurations. To review resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](#iam-resource-types). Possible values include `location`, `link`, `configuration`, `cluster`, `clustergroup`, and `subscription`. If you scope an access policy to the `location` resource type, the users must target the regional endpoint to interact with the location. For more information, see the [troubleshooting topic](/docs/satellite?topic=satellite-ts-location-missing-location). |
    | Resource instance  \n CLI option: `--resource` | If you scope the policy to a resource type, you can further limit the policy to a particular instance of the resource. To list available instances, run [the CLI commands](/docs/satellite?topic=satellite-satellite-cli-reference) for that resource type, such as `ibmcloud sat location ls`.  To grant permissions to create a location, do not include the `--resource` option, which limits access to only a particular location. Note that you cannot scope a policy to individual `configuration` or `subscription` resources. Instead, control access to your {{site.data.keyword.satelliteshort}} Config resources at the `clustergroup` level. |
    | Role  \n CLI option: `--role` | Choose the platform or service access that you want to assign. \n * Platform: Grants access to {{site.data.keyword.satelliteshort}} platform resources so that users can manage infrastructure resources such as locations, hosts, or link endpoints. For more information, see [Platform access roles](#iam-roles-platform). Possible values are `Administrator`, `Operator`, `Editor`, or `Viewer`. \n * Service: Grants access to services that run within {{site.data.keyword.satelliteshort}} resources so that users can work with {{site.data.keyword.satelliteshort}} Config subscriptions and Kubernetes resources. For more information, see [Service access roles](#iam-roles-service). Possible values are `Manager`, `Writer`, or `Reader`. |
    {: caption="Table 1. Options to scope the access policy." caption-side="top"} 

3. Verify that the user or access group has the assigned role.
    - For individual users
    
        ```sh
        ibmcloud iam user-policies <user@email.com>
        ```
        {: pre}

    - For access groups
    
        ```sh
        ibmcloud iam access-group-policies <access_group>
        ```
        {: pre}

## IAM platform and service roles
{: #iam-roles}

Every user that accesses the {{site.data.keyword.satelliteshort}} service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.
{: shortdesc}

The name for the {{site.data.keyword.satellitelong_notm}} service in IAM is
- **{{site.data.keyword.satellitelong_notm}}** in the UI
- **satellite** in the API and CLI

Keep in mind that you need permissions to {{site.data.keyword.cloud_notm}} services if you use the services with {{site.data.keyword.satelliteshort}}. For example, to create and manage clusters in your {{site.data.keyword.satelliteshort}} location, you must have the [appropriate permissions to {{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-access_reference) in IAM (**Kubernetes Service** in the UI, **containers-kubernetes** in the API and CLI).
{: note}

### Access policies
{: #iam-roles-policies}

Policies enable access at different levels. Some options for {{site.data.keyword.satellitelong_notm}} include the following.
{: shortdesc}

- Access across all {{site.data.keyword.satelliteshort}} service instances of all resource types in your account.
- Access to specific resource types within {{site.data.keyword.satelliteshort}}. For more information about resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](#iam-resource-types).
    - **Location** in the UI, **location** in the API and CLI. (When scoped, users must [target the regional endpoint](/docs/satellite?topic=satellite-ts-location-missing-location).)
    - **Link** in the UI, **link** in the API and CLI.
    - {{site.data.keyword.satelliteshort}} Config resource types:
        - **Cluster** in the UI, **cluster** in the API and CLI.
        - **Clustergroup** in the UI, **clustergroup** in the API and CLI.
        - **Configuration** in the UI, **configuration** in the API and CLI.
        - **Subscription** in the UI, **subscription** in the API and CLI.

- Access to an individual resource of a particular resource type, such as a particular location {{site.data.keyword.satelliteshort}}. The following resource types can be scoped to particular instances.
    - **Location** in the UI, **location** in the API and CLI. (When scoped, users must [target the regional endpoint](/docs/satellite?topic=satellite-ts-location-missing-location).)
    - **Link** in the UI, **link** in the API and CLI.
    - {{site.data.keyword.satelliteshort}} Config resource types:
        - **Cluster** in the UI, **cluster** in the API and CLI.
        - **Clustergroup** in the UI, **clustergroup** in the API and CLI.

After you define the scope of the access policy, you assign a role, which determines the user's level of access. Review the following sections that outline what actions each platform and service role allows within the {{site.data.keyword.satelliteshort}} service.

### Platform access roles
{: #iam-roles-platform}

Click the tabs in the following table to review the actions that are mapped to platform access roles for different components of {{site.data.keyword.satelliteshort}}. Platform access roles enable users to perform tasks on service resources at the platform level, for example, assign user access for the service, create or delete instances, and bind instances to applications.
{: shortdesc}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Create a {{site.data.keyword.satelliteshort}} location. | `/createController` | [`location create`](/docs/satellite?topic=satellite-satellite-cli-reference#location-create) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| List {{site.data.keyword.satelliteshort}} locations. | `/getControllers` | [`location ls`](/docs/satellite?topic=satellite-satellite-cli-reference#location-ls) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Get the details of a {{site.data.keyword.satelliteshort}} location. | `/getController` | [`location get`](/docs/satellite?topic=satellite-satellite-cli-reference#location-get) | |![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |  
| Remove a {{site.data.keyword.satelliteshort}} location. | `/removeController` | [`location rm`](/docs/satellite?topic=satellite-satellite-cli-reference#location-rm) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Attach a host to a {{site.data.keyword.satelliteshort}} location. | `/hostqueue/createRegistrationScript` | [`host attach`](/docs/satellite?topic=satellite-satellite-cli-reference#host-attach) | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Assign a host to a {{site.data.keyword.satelliteshort}} location control plane or cluster. | `/hostqueue/createAssignment`| [`host assign`](/docs/satellite?topic=satellite-satellite-cli-reference#host-assign) | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| List hosts in a {{site.data.keyword.satelliteshort}} location. | `/hostqueue/getHosts` | [`host ls`](/docs/satellite?topic=satellite-satellite-cli-reference#host-ls) | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg)|![Feature available.](images/icon-checkmark-filled.svg)|![Feature available.](images/icon-checkmark-filled.svg)|
| Update a host. | `/hostqueue/updateHost` | [`host update`](/docs/satellite?topic=satellite-satellite-cli-reference#host-update) | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a host from a {{site.data.keyword.satelliteshort}} location control plane or cluster.| `/hostqueue/removeHost` |[`host rm`](/docs/satellite?topic=satellite-satellite-cli-reference#host-rm) | | | ![Feature available.](images/icon-checkmark-filled.svg)|![Feature available.](images/icon-checkmark-filled.svg)|![Feature available.](images/icon-checkmark-filled.svg)|
{: class="comparison-tab-table"}
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}
{: #platform-table1}
{: tab-title="Locations and hosts"}
{: tab-group="iam-platform"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different platform roles are in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| List {{site.data.keyword.satelliteshort}} endpoints for a location. | `GET /v1/locations/{location_id}/endpoints` | [`ibmcloud sat endpoint ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-ls) | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Get the details of a {{site.data.keyword.satelliteshort}} endpoint. | `GET /v1/locations/{location_id}/endpoints/{endpoint_id}` | [`ibmcloud sat endpoint get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-get) | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Create a {{site.data.keyword.satelliteshort}} endpoint. | `POST /v1/locations/{location_id}/endpoints/` | [`ibmcloud sat endpoint create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-create) | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Update a {{site.data.keyword.satelliteshort}} endpoint. | `PATCH /v1/locations/{location_id}/endpoints/{endpoint_id}` | [`ibmcloud sat endpoint update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-update) | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Delete a {{site.data.keyword.satelliteshort}} endpoint. | `DELETE /v1/locations/{location_id}/endpoints/{endpoint_id}` | [`ibmcloud sat endpoint rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-rm) | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Get the destination certificate of a {{site.data.keyword.satelliteshort}} endpoint. |`GET /v1/locations/{location_id}/endpoints/{endpoint_id}/cert`| | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Upload a destination certificate for a {{site.data.keyword.satelliteshort}} endpoint. | `POST /v1/locations/{location_id}/endpoints/{endpoint_id}/cert` | | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Delete the destination certificate of a {{site.data.keyword.satelliteshort}} endpoint. | `DELETE /v1/locations/{location_id}/endpoints/{endpoint_id}/cert` | | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| View the source list for a {{site.data.keyword.satelliteshort}} endpoint. | `GET /v1/locations/{location_id}/endpoints/{endpoint_id}/sources` | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Update the source list for a {{site.data.keyword.satelliteshort}} endpoint. | `PATCH /v1/locations/{location_id}/endpoints/{endpoint_id}/sources` | | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| List the sources for all {{site.data.keyword.satelliteshort}} endpoints. | `GET /v1/locations/{location_id}/sources` | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Create a {{site.data.keyword.satelliteshort}} endpoint source. | `POST /v1/locations/{location_id}/sources`| | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Update a {{site.data.keyword.satelliteshort}} endpoint source. | `PATCH /v1/locations/{location_id}/sources/{source_id}`| | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Delete a {{site.data.keyword.satelliteshort}} endpoint source. | `DELETE /v1/locations/{location_id}/sources/{source_id}`| | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Export the configuration for a {{site.data.keyword.satelliteshort}} endpoint to a file. | `GET ​/v1​/locations​/{location_id}​/endpoints​/{endpoint_id}​/export`	| | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Import the configuration for a {{site.data.keyword.satelliteshort}} endpoint from a file. | `POST /v1/locations/{location_id}/endpoints/import` | | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| List the {{site.data.keyword.satelliteshort}} endpoints that a client (source) is configured for and the enabled status of the client (source) for each endpoint. |	`GET /v1​/locations​/{location_id}​/sources​/{source_id}​/endpoints` | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Enable or disable a client (source) for one or more {{site.data.keyword.satelliteshort}} endpoints.	| `PATCH ​/v1​/locations​/{location_id}​/sources​/{source_id}​/endpoints` | | | | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}
{: #platform-table2}
{: tab-title="Link"}
{: tab-group="iam-platform"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different platform roles are in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Create a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `addChannel`|[`config create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-create) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Delete a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. |`removeChannel` |[`config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rm) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| View an organization in {{site.data.keyword.satelliteshort}} Config. | `organization`| - | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}
{: #platform-table3}
{: tab-title="Configuration"}
{: tab-group="iam-platform"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different platform roles are in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Set the configuration version that a cluster group is subscribed to in {{site.data.keyword.satelliteshort}} Config. | `setSubscription`| [`subscription update --version`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update)| | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}
{: #platform-table4}
{: tab-title="Subscription"}
{: tab-group="iam-platform"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different platform roles are in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Attach a cluster to {{site.data.keyword.satelliteshort}} Config. | `registerCluster` | [`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register)| | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Get a script to register a cluster to {{site.data.keyword.satelliteshort}} Config. | `enableRegistrationUrl`|[`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a cluster from {{site.data.keyword.satelliteshort}} Config. | `deleteCluster`|[`cluster unregister`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-unregister) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| View clusters that are attached to {{site.data.keyword.satelliteshort}} Config. |`clusters` calls |[`cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls)  \n  [`cluster get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-get) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}
{: #platform-table5}
{: tab-title="Cluster"}
{: tab-group="iam-platform"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different platform roles are in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Add clusters to a cluster group.|`groupClusters`|[`cluster-group attach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
| Remove clusters from a cluster group.|`unGroupClusters`|[`cluster-group detach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-detach)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
|Create a cluster group.|`addGroup`|[`cluster-group create`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-create)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
| View cluster groups that are attached to {{site.data.keyword.satelliteshort}} Config. |`groups` |[`cluster-group ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-ls)   \n [`cluster-group get`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-get) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a cluster group. |`removeGroup` |[`cluster-group rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-rm) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}
{: #platform-table6}
{: tab-title="Clustergroup"}
{: tab-group="iam-platform"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different platform roles are in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| View Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `resources` calls | `resource ls`| | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}
{: #platform-table7}
{: tab-title="Resource"}
{: tab-group="iam-platform"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different platform roles are in the following columns: none, viewer, editor, operator, and administrator."}

`*` You cannot scope access policies to a particular {{site.data.keyword.satelliteshort}} Config **resource**. Instead, scope the policy to the {{site.data.keyword.satellitelong_notm}} service so that users can list {{site.data.keyword.satelliteshort}} Config resources.
{: note}

### Service access roles
{: #iam-roles-service}

Click the tabs in the following table to review the actions that are mapped to service access roles for different components of {{site.data.keyword.satelliteshort}}. Service access roles enable users access to {{site.data.keyword.satelliteshort}} and the ability to call the {{site.data.keyword.satelliteshort}} APIs.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Config uses a [custom IAM service access role](/docs/account?topic=account-custom-roles), **Deployer**, in addition to the standard **Reader**, **Writer**, and **Manager** roles. You can assign users the **Deployer** role so that they can deploy existing configurations to your clusters, but cannot add or edit the actual configurations for your apps.
{: note}

| Action | API | CLI | None | Reader | Writer | Manager | {{site.data.keyword.satelliteshort}} Link Administrator | {{site.data.keyword.satelliteshort}} Link Source Access Controller |
|-----|---|---|-----|-----|-----|--------|---|---|
| Export the configuration for a {{site.data.keyword.satelliteshort}} endpoint to a file. | `GET ​/v1​/locations​/{location_id}​/endpoints​/{endpoint_id}​/export`	| | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |	|
| Import the configuration for a {{site.data.keyword.satelliteshort}} endpoint from a file. | `POST /v1/locations/{location_id}/endpoints/import` | | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |	|
| List the {{site.data.keyword.satelliteshort}} endpoints that a client (source) is configured for and the enabled status of the client (source) for each endpoint. | `GET /v1​/locations​/{location_id}​/sources​/{source_id}​/endpoints` | | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |	|
| Enable or disable a client (source) for one or more {{site.data.keyword.satelliteshort}} endpoints.	| `PATCH ​/v1​/locations​/{location_id}​/sources​/{source_id}​/endpoints` | | | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with service access roles." caption-side="bottom"}
{: #service-table1}
{: tab-title="Link"}
{: tab-group="iam-service"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different service roles are in the following columns: none, reader, writer, deployer, and manager."}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| View a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `channel` |[`configuration get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-get)  /n  [`configuration ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-ls)| | ![Feature available.](images/icon-checkmark-filled.svg) | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Create a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `addChannel` | [`configuration create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-create)| | |  | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Update a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `editChannel`| [`configuration rename`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rename)| | | ![Feature available.](images/icon-checkmark-filled.svg) | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Delete a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `removeChannel` | [`configuration rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rm)| | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Add a Kubernetes resource definition as a version to a {{site.data.keyword.satelliteshort}} configuration. | `addChannelVersion`| [`configuration version create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-create) | | | ![Feature available.](images/icon-checkmark-filled.svg) |  | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a version from a {{site.data.keyword.satelliteshort}} configuration. | `removeChannelVersion`| [`configuration version rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-create) | | | ![Feature available.](images/icon-checkmark-filled.svg) |  | ![Feature available.](images/icon-checkmark-filled.svg) |
| Get details for a version that you added to a {{site.data.keyword.satelliteshort}} configuration. | `getChannelVersion`| [`configuration version get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-get) | | | ![Feature available.](images/icon-checkmark-filled.svg) | | ![Feature available.](images/icon-checkmark-filled.svg) |
| View an organization in {{site.data.keyword.satelliteshort}} Config. |`organization` | - | | | ![Feature available.](images/icon-checkmark-filled.svg) | |![Feature available.](images/icon-checkmark-filled.svg) | ![Feature available.](images/icon-checkmark-filled.svg) |
| Manage an organization in {{site.data.keyword.satelliteshort}} Config | - | | | | |  | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with service access roles." caption-side="bottom"}
{: #service-table2}
{: tab-title="Configuration"}
{: tab-group="iam-service"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different service roles are in the following columns: none, reader, writer, deployer, and manager."}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| View a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `subscription`| [`subscription get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-get)   \n  [`subscription ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-ls)| |![Feature available.](images/icon-checkmark-filled.svg) | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
| Create a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `addSubscription`| [`subscription create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-create)| | | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
| Update a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `editSubscription`| [`subscription update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update)| | | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
| Set the configuration version of Kubernetes resources for a subscription in {{site.data.keyword.satelliteshort}} Config. | `setSubscription`|[`subscription update --version`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update) | | | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
| Delete a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. |`removeSubscription` |[`subscription rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-rm) | | | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
{: class="comparison-tab-table"}
{: caption="Actions that you can take with service access roles." caption-side="bottom"}
{: #service-table3}
{: tab-title="Subscription"}
{: tab-group="iam-service"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different service roles are in the following columns: none, reader, writer, deployer, and manager."}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| Attach a cluster to {{site.data.keyword.satelliteshort}} Config. | `registerCluster` | [`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register) | | | |  |![Feature available.](images/icon-checkmark-filled.svg) |
| Get a script to register a cluster to {{site.data.keyword.satelliteshort}} Config. | `enableRegistrationUrl`|[`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a cluster from {{site.data.keyword.satelliteshort}} Config. | `deleteCluster`|[`cluster unregister`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-unregister) | | | |  |![Feature available.](images/icon-checkmark-filled.svg) |
| View clusters that are attached to {{site.data.keyword.satelliteshort}} Config. | `clusters` calls |[`cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls)   \n  [`cluster get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-get) | |![Feature available.](images/icon-checkmark-filled.svg) | |  |![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with service access roles." caption-side="bottom"}
{: #service-table4}
{: tab-title="Cluster"}
{: tab-group="iam-service"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different service roles are in the following columns: none, reader, writer, deployer, and manager."}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| Add clusters to a cluster group.|`groupClusters`|[`cluster-group attach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
| Remove clusters from a cluster group.|`unGroupClusters`|[`cluster-group detach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-detach)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
|Create a cluster group.|`addGroup`|[`cluster-group create`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-create)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
| View cluster groups that are attached to {{site.data.keyword.satelliteshort}} Config. |`groups` |[`cluster-group ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-ls)   \n [`cluster-group get`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-get) | | ![Feature available.](images/icon-checkmark-filled.svg)| | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a cluster group. |`removeGroup` |[`cluster-group rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-rm) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with service access roles." caption-side="bottom"}
{: #service-table5}
{: tab-title="Clustergroup"}
{: tab-group="iam-service"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different service roles are in the following columns: none, reader, writer, deployer, and manager."}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| View Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `resources` calls | [`resource ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-ls)   \n  [`resource get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-get) | | ![Feature available.](images/icon-checkmark-filled.svg) | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: class="comparison-tab-table"}
{: caption="Actions that you can take with service access roles." caption-side="bottom"}
{: #service-table6}
{: tab-title="Resources"}
{: tab-group="iam-service"}
{: row-headers}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right. The action is in the first column. The API for the action is in the second column. The CLI for the action is in the third column. The different service roles are in the following columns: none, reader, writer, deployer, and manager."}

`*` You cannot scope access policies to a particular {{site.data.keyword.satelliteshort}} Config **resource**. Instead, scope the policy to the {{site.data.keyword.satellitelong_notm}} service so that users can list {{site.data.keyword.satelliteshort}} Config resources.
{: note}

### Platform and service roles for {{site.data.keyword.redhat_openshift_notm}} clusters
{: #iam-roles-clusters}

If you create {{site.data.keyword.openshiftlong_notm}} clusters to use in your {{site.data.keyword.redhat_openshift_notm}} locations, you manage access to these clusters in IAM for the {{site.data.keyword.redhat_openshift_notm}} service, not for {{site.data.keyword.redhat_openshift_notm}}. Review the following information to manage IAM access to {{site.data.keyword.redhat_openshift_notm}} clusters.
{: shortdesc}

- [Reference documentation](/docs/openshift?topic=openshift-access_reference) for user access permissions including [platform](/docs/openshift?topic=openshift-access_reference#iam_platform) and [service](/docs/openshift?topic=openshift-access_reference#service) roles.
- [Assigning access to clusters](/docs/openshift?topic=openshift-users), such as setting up the API key for underlying infrastructure permissions and granting users access with {{site.data.keyword.cloud_notm}} IAM.
- [Accessing clusters](/docs/openshift?topic=openshift-access_cluster) on the public or private service endpoints, or by using an {{site.data.keyword.cloud_notm}} IAM API key such as for automation purposes.

## Common use cases and roles in {{site.data.keyword.cloud_notm}}
{: #iam-roles-usecases}

Wondering which access roles to assign to your {{site.data.keyword.satelliteshort}} access groups and users? Use the examples in following table to determine which roles and scope to assign.
{: shortdesc}

| Use case | Example roles and scope |
| --- | --- |
| Creating a location | The user and the [API key that is set for the region and resource group](/docs/openshift?topic=openshift-access-creds#api_key_about) require the following permissions. **Administrator** platform role for all {{site.data.keyword.satelliteshort}} locations. The custom **{{site.data.keyword.satelliteshort}} Link Administrator** service role for {{site.data.keyword.satelliteshort}} Link. **Manager** service role to the {{site.data.keyword.cos_full_notm}} instance that backs up the location control plane data. To use automated templates such as to add hosts from AWS or Azure, the **Administrator** platform role for {{site.data.keyword.bplong_notm}} and **Administrator** platform role for Kubernetes Service. For additional permissions to set up the location control plane, see [Permissions to create a cluster](/docs/openshift?topic=openshift-access_reference#cluster_create_permissions). |
| Creating a cluster in a location | See [Permissions to create a cluster](/docs/openshift?topic=openshift-access_reference#cluster_create_permissions). |
| Location auditor | **Viewer** platform role for the {{site.data.keyword.satelliteshort}} location and link endpoints. **Reader** service role for the configuration resources in the location. **Reader** service role to the {{site.data.keyword.cos_full_notm}} instance that backs up the location control plane data. |
| App developers | **Viewer** platform role for the {{site.data.keyword.satelliteshort}} location. **Writer** or **Deployer** service access role for the configuration resources. **Editor** platform role and **Writer** service role to {{site.data.keyword.redhat_openshift_notm}} clusters or particular projects in a cluster.|
| Billing | **Viewer** platform role for all the {{site.data.keyword.satelliteshort}} locations in the account. |
| Location administrator | **Administrator** platform role for the location and link resources. **Administrator** platform role to {{site.data.keyword.redhat_openshift_notm}} clusters. **Manager** service role to the {{site.data.keyword.cos_full_notm}} instance that backs up the location control plane data.|
| DevOps operator | **Editor** platform role for the location and link resources. **Deployer** service role for the configurations. **Operator** platform role to {{site.data.keyword.redhat_openshift_notm}} clusters.|
| Operator or site reliability engineer | **Administrator** platform role for the location and link resources. **Manager** service role for the configuration resources. **Administrator** platform role and **Manager** service role to {{site.data.keyword.redhat_openshift_notm}} clusters. |
{: caption="Types of roles you might assign to meet different use cases." caption-side="bottom"}
{: summary="The first column contains the use case, which is typically the role of an access group or user. The second column is the example role and scope of the role that you assign the user in {{site.data.keyword.cloud_notm}} IAM."}

## API keys in {{site.data.keyword.cloud_notm}}
{: #sat-api-keys}

{{site.data.keyword.satelliteshort}} uses [API keys](/docs/account?topic=account-manapikey) from {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) to authorize various requests.
{: shortdesc}

### Container service API key
{: #api-keys-containers}

{{site.data.keyword.satelliteshort}} uses the API key that is set for the container service, {{site.data.keyword.openshiftlong_notm}}, which is specific to the resource group and region that the {{site.data.keyword.satelliteshort}} location is managed from.
{: shortdesc}

The API key name is in the format `containers-kubernetes-key`. The account owner can reset the API key by logging in to a region and resource group and running `ibmcloud ks api-key reset`.

This API key is used to authorize actions to various {{site.data.keyword.cloud_notm}} services, such as one of the following.
- {{site.data.keyword.openshiftlong_notm}} for clusters.
- {{site.data.keyword.registrylong_notm}} for images.
- Service-to-service authorization in IAM for any {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services that you add to your location.

For more information, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-access-creds#api_key_about).

### Template API key
{: #api-keys-templates}

If you create a {{site.data.keyword.satelliteshort}} location by using a template, such as for AWS, {{site.data.keyword.satelliteshort}} checks for permissions by using an API key. The API key must have the [required permissions to create a location](#iam-roles-usecases), including to {{site.data.keyword.bplong_notm}} which is used to automate the infrastructure creation from the template's cloud provider.
{: shortdesc}

By default, {{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM API key for you, that impersonates the permissions of the user that tries to use the template. The API key name is formatted as `satellite-<location_name>`.

When you use a template, you can optionally provide the value of an existing API key that has the correct permissions in the same account.

## Common permissions in other cloud providers
{: #permissions-other-clouds}

To create and manage the underlying infrastructure in other cloud providers, you must have the appropriate permissions. Some commonly required permissions are listed in the following section. For more information, consult your cloud provider's documentation.
{: shortdesc}

### Alibaba permissions
{: #permissions-alibaba}

To allow users in Alibaba to do various actions for {{site.data.keyword.satelliteshort}}, you can grant the users access to the **AliyunECSFullAccess** security policy or create a custom security policy that allows access to only specific instances. For more information about the permissions of this role, see the [Alibaba documentation](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/ram-overview){: external}.
{: shortdesc}

### AWS permissions
{: #permissions-aws}

Review the following example policies that you might give users in AWS to do various actions for {{site.data.keyword.satelliteshort}}. If you want to further restrict permissions, consult the AWS documentation.
{: shortdesc}

#### Manually creating a {{site.data.keyword.satelliteshort}} location in AWS
{: #permissions-aws-manual}

- `AmazonEC2FullAccess`
- `AmazonElasticFileSystemFullAccess`
- `AmazonVPCFullAccess`
- `AWSMarketplaceFullAccess`

#### Automatically creating a {{site.data.keyword.satelliteshort}} location from a {{site.data.keyword.bpshort}} template in AWS
{: #permissions-aws-auto}

- `AmazonEC2FullAccess`
- `AmazonElasticFileSystemFullAccess`
- `AmazonSSMFullAccess`
- `AmazonVPCFullAccess`
- `AWSMarketplaceFullAccess`
- `IAMFullAccess`

### Azure permissions
{: #permissions-azure}

To allow users in Microsoft Azure to do various actions for {{site.data.keyword.satelliteshort}}, you can grant the users the general **Contributor** built-in role in Azure role-based access control. For more information about further restricting permissions, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles){: external}.
{: shortdesc}

### Google Cloud Platform permissions
{: #permissions-gcp}

To allow users in Google Cloud Platform to do various actions for {{site.data.keyword.satelliteshort}}, you can grant the users the **Editor** role to the project in GCP IAM. For more information about the permissions of this role, see the [GCP documentation](https://cloud.google.com/iam/docs/permissions-reference){: external}.
{: shortdesc}




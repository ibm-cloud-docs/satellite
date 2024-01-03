---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud, assign access, access for satellite

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Assigning access with {{site.data.keyword.cloud_notm}} IAM
{: #iam-assign-access}

To grant access to {{site.data.keyword.satelliteshort}} resources, use {{site.data.keyword.cloud_notm}} IAM. For information about assigning user roles in the console, see [Managing access to resources](/docs/account?topic=account-assign-access-resources).
{: shortdesc}

## Access policies
{: #iam-roles-policies}

Policies enable access at different levels. Some options for {{site.data.keyword.satellitelong_notm}} include the following.
{: shortdesc}

- Access across all {{site.data.keyword.satelliteshort}} service instances of all resource types in your account.
- Access to specific resource types within {{site.data.keyword.satelliteshort}}. For more information about resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](/docs/satellite?topic=satellite-iam).
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

## Overview of the process to set up access to {{site.data.keyword.satellitelong_notm}} in {{site.data.keyword.cloud_notm}} IAM
{: #iam-assign-overview}

As a general practice, you can invite users to your {{site.data.keyword.cloud_notm}} account, add them to an access group, and assign them access to {{site.data.keyword.satellitelong_notm}} resources in IAM. You might also add access policies for other {{site.data.keyword.cloud_notm}} services, or assign individual user access.
{: shortdesc}

1. [Invite users to your account](/docs/account?topic=account-iamuserinv).
2. [Create an access group](/docs/account?topic=account-groups&interface=ui#create_ag) to add users to.
3. [Assign the access group](/docs/account?topic=account-groups&interface=ui#access_ag) with the appropriate scope for the {{site.data.keyword.satelliteshort}} resources and IAM platform and service roles for the actions you want to let users in your access group perform.
    - To scope access to the service, use **{{site.data.keyword.satellitelong_notm}}** in the UI or **satellite** in the API or CLI.
    - You can scope access to the account or particular resource groups. Keep in mind the following points.
        - Account-level access is not the same as access to all resource groups.
        - Not all {{site.data.keyword.satelliteshort}} resource types support scoping to resource groups. For example, you cannot scope {{site.data.keyword.satelliteshort}} Config resource types (configuration, subscription, cluster, or cluster group) or {{site.data.keyword.satelliteshort}} storage service to resource groups, only to the account.
    - For help with scoping the role to the right {{site.data.keyword.satelliteshort}} resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](/docs/satellite?topic=satellite-iam). You can scope access policies to the following resource types.
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
        - [Platform access roles](/docs/satellite?topic=satellite-iam-platform-access)
        - [Service access roles](/docs/satellite?topic=satellite-iam-service-access)
        - [Common use cases and roles](/docs/satellite?topic=satellite-iam#iam-roles-usecases)
    - Consider creating a **Reader** service policy to {{site.data.keyword.satellitelong_notm}} (and not scoped to a particular resource type or resource) so that users can view the {{site.data.keyword.satelliteshort}} Config resources that run in {{site.data.keyword.satelliteshort}} clusters, such as pods or deployments.
4. [Assign the access group](/docs/account?topic=account-groups&interface=ui#access_ag) with the appropriate scope for any other {{site.data.keyword.cloud_notm}} services that you plan to use in your {{site.data.keyword.satelliteshort}} location. Refer to each service documentation for the level of access that you need. Common services include:
    - {{site.data.keyword.openshiftlong_notm}} clusters: **Kubernetes Service** in the UI, **containers-kubernetes** in the API and CLI.
    - {{site.data.keyword.registrylong_notm}} for a private registry across clusters: **Container Registry** in the UI, **container-registry** in the API and CLI.
    - {{site.data.keyword.cos_full_notm}} for the backing storage for your location information: **Cloud Object Storage** in the UI, **cos** in the API and CLI.
5. [Assign the access group](/docs/account?topic=account-groups&interface=ui#access_ag) with the **Viewer** platform access role to any resource groups that you plan to use with {{site.data.keyword.satelliteshort}}.


## Assigning access policy to access group by using the console
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
9. For **Platform access**, select the **Editor** role so that all users in your access group can add and remove hosts and endpoints from the {{site.data.keyword.satelliteshort}} location, but cannot create or delete locations. For other roles by resource type, see [IAM platform](/docs/satellite?topic=satellite-iam-platform-access) and [IAM service](/docs/satellite?topic=satellite-iam-service-access) roles.
10. Click **Add+**.
11. In the **Access summary** pane, review the access policy, and then click **Assign**.
12. From the access group **Access policies** table, verify that the Editor policy is added to the access group.


## Assigning access policy to access group with the CLI
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
    | {{site.data.keyword.satelliteshort}} resource   \n CLI option: `--resource-type` | You can limit the policy to a type of resource within {{site.data.keyword.satellitelong_notm}}, such as all {{site.data.keyword.satelliteshort}} locations or {{site.data.keyword.satelliteshort}} configurations. To review resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](/docs/satellite?topic=satellite-iam). Possible values include `location`, `link`, `configuration`, `cluster`, `clustergroup`, and `subscription`. If you scope an access policy to the `location` resource type, the users must target the regional endpoint to interact with the location. For more information, see the [troubleshooting topic](/docs/satellite?topic=satellite-ts-location-missing-location). |
    | Resource instance  \n CLI option: `--resource` | If you scope the policy to a resource type, you can further limit the policy to a particular instance of the resource. To list available instances, run [the CLI commands](/docs/satellite?topic=satellite-satellite-cli-reference) for that resource type, such as `ibmcloud sat location ls`.  To grant permissions to create a location, do not include the `--resource` option, which limits access to only a particular location. Note that you cannot scope a policy to individual `configuration` or `subscription` resources. Instead, control access to your {{site.data.keyword.satelliteshort}} Config resources at the `clustergroup` level. |
    | Role  \n CLI option: `--role` | Choose the platform or service access that you want to assign. \n * Platform: Grants access to {{site.data.keyword.satelliteshort}} platform resources so that users can manage infrastructure resources such as locations, hosts, or link endpoints. For more information, see [Platform access roles](/docs/satellite?topic=satellite-iam-platform-access). Possible values are `Administrator`, `Operator`, `Editor`, or `Viewer`. \n * Service: Grants access to services that run within {{site.data.keyword.satelliteshort}} resources so that users can work with {{site.data.keyword.satelliteshort}} Config subscriptions and Kubernetes resources. For more information, see [Service access roles](/docs/satellite?topic=satellite-iam-service-access). Possible values are `Manager`, `Writer`, or `Reader`. |
    {: caption="Table 1. Options to scope the access policy." caption-side="bottom"} 

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
        
        
## Checking user permissions
{: #checking-perms}

Before you complete a task, you might want to check that you have the appropriate permissions in {{site.data.keyword.cloud}} Identity and Access Management (IAM).
{: shortdesc}

### Checking IAM platform and service access roles from the UI
{: #checking-iam-ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} IAM console](https://cloud.ibm.com/iam){: external}.
2. From the navigation menu, click the **Users** tab.
3. In the table, click the user with the tag `self` for yourself or the user that you want to check.
4. Click the **Access policies** tab.
5. Review the **Resource attributes** column for a short description of the access. Click the number tag to view all the allowed actions for the role.
6. To review what the roles and allowed actions permit, see [Platform access roles](/docs/satellite?topic=satellite-iam-platform-access) and [Service access roles](/docs/satellite?topic=satellite-iam-service-access). 
7. To change or assign new access policies, see [Assigning {{site.data.keyword.satelliteshort}} access](#iam-assign-overview).


### Checking IAM platform and service access roles from the CLI
{: #checking-iam-cli}

1. Log in to your {{site.data.keyword.cloud_notm}} account. If you have a federated ID, include the `--sso` option.
    ```sh
    ibmcloud login -r [--sso]
    ```
    {: pre}

2. Find the **User ID** of the user whose permissions you want to check.
    ```sh
    ibmcloud account users
    ```
    {: pre}

3. Check the IAM access policies of the user.
    ```sh
    ibmcloud iam user-policies <user_id>
    ```
    {: pre}

4. To review what the roles and allowed actions permit, see [Platform access roles](/docs/satellite?topic=satellite-iam-platform-access) and [Service access roles](/docs/satellite?topic=satellite-iam-service-access). 
5. To change or assign new access policies, see [Assigning {{site.data.keyword.satelliteshort}} access](#iam-assign-overview).





---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-13"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}


# Managing access for {{site.data.keyword.satelliteshort}}
{: #iam}

Access to {{site.data.keyword.satellitelong}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).
{: shortdesc}

The name for the {{site.data.keyword.satellitelong}} service in IAM is:
* **IBM Cloud Satellite** in the UI
* **satellite** in the API and CLI

## Understanding {{site.data.keyword.satelliteshort}} resource types for access
{: #iam-resource-types}

You can use {{site.data.keyword.cloud_notm}} IAM to assign access to [different resources in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-service-architecture). The way that access works varies depending on the resource that you want to authorize.
{: shortdesc}

| {{site.data.keyword.satelliteshort}} component | Resource type | IAM role | Scope | Description |
| ------------- | ------------- | ---------------- | ----- | ----------- |
|{{site.data.keyword.satelliteshort}} Location| | Location| Platform | Account, resource group, or particular instances | [Locations](/docs/satellite?topic=satellite-locations) are places that you use to extend {{site.data.keyword.cloud_notm}} by attaching your own host compute machines to the location. Access to a location also gives access to manage hosts in the location, but does not grant access to other resources that run within the location, such as configurations, link endpoints, and {{site.data.keyword.openshiftshort}} clusters. |
|{{site.data.keyword.satelliteshort}} Config | Configuration </br> Subscription </br> Cluster </br> Clustergroup </br> Resource | Platform, Service | Account, resource group, {{site.data.keyword.satelliteshort}} location, or particular instances | {{site.data.keyword.satelliteshort}} Config is a collection of configurations, versions, and subscriptions that you use to automatically deploy Kubernetes resources to groups of clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component. Note that access to {{site.data.keyword.satelliteshort}} Config does not give a user access to the clusters that run the Kubernetes resources of the configuration. You can scope access to the following {{site.data.keyword.satelliteshort}} Config resources:<ul><li>**Configurations**, where you upload the version of the configuration file for the Kubernetes resources that you want to deploy.</li><li>**Subscriptions**, which you use to use to specify the cluster group where you want to deploy the Kubernetes resource definition that you added as a version to your configuration.</li><li>**Clusters** or **cluster groups**, which are {{site.data.keyword.openshiftlong_notm}} that are registered with {{site.data.keyword.satelliteshort}} Config and can be subscribed to configurations.</li><li>**Resources**, which are Kubernetes resources such as pods or services that are described in a {{site.data.keyword.satelliteshort}} Config and run in a subscribed cluster.</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | Link | Platform, Service | Account, resource group, or particular instances | Link endpoints connect services, servers, or apps that run in your {{site.data.keyword.satelliteshort}} location with an endpoint that runs in {{site.data.keyword.cloud_notm}}. Note that access to a {{site.data.keyword.satelliteshort}} Link does not give a user access to the resources that the endpoint connects, such as a location or service instance. |
| N/A| {{site.data.keyword.openshiftshort}} clusters | Platform and service | Account, resource group, {{site.data.keyword.cloud_notm}} region, or particular instances | You do not assign access policies for {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}. Instead, access to clusters is assigned in {{site.data.keyword.cloud_notm}} IAM through {{site.data.keyword.openshiftlong_notm}} (**Kubernetes Service** in the console or `containers-kubernetes` in the API or CLI). For more information, see [Platform and service roles for {{site.data.keyword.openshiftshort}} clusters](#iam-roles-clusters). If you have access to a {{site.data.keyword.satelliteshort}} location or configuration, you can view the clusters that are attached to the location or configuration, but you might not be able to access the clusters if you do not have the appropriate roles to those clusters. For example, if you have the appropriate access to a {{site.data.keyword.satelliteshort}} configuration, you might be able to list all the Kubernetes resources that run in registered clusters via the {{site.data.keyword.satelliteshort}} Config API. However, without an access policy to the individual clusters, you cannot log in to the individual clusters and use {{site.data.keyword.openshiftshort}} APIs to list Kubernetes resources. |
|N/A| Other {{site.data.keyword.satelliteshort}}-enabled services | Varies by service | Varies by service | Similar to {{site.data.keyword.openshiftshort}} clusters, authorization to other {{site.data.keyword.cloud_notm}} services that are enabled in {{site.data.keyword.satelliteshort}} is managed in {{site.data.keyword.cloud_notm}} IAM through those services. For more information, refer to each service's documentation. |
{: caption="Resource types that you can manage access to" caption-side="top"}
{: summary="The table shows information about the resource types that you can manage access to. Rows are read from the left to right. The first column is the resource type. The second column is the type of IAM role with which you assign access to the resource type. The third column is the scope that the access can be constrained to. The fourth column is a description of the resource type and access considerations."}

## Assigning access with {{site.data.keyword.cloud_notm}} IAM
{: #iam-assign}

For information about assigning user roles in the console, see [Managing access to resources](/docs/account?topic=account-assign-access-resources#iammanidaccser).
{: shortdesc}

**Process to set up access to {{site.data.keyword.satellitelong_notm}} in your {{site.data.keyword.cloud_notm}} account:**<br>

1.  [Invite users to your account](/docs/account?topic=account-iamuserinv).
2.  [Create an access group](/docs/account?topic=account-groups#create_ag) to add users to.
3.  [Assign the access group](/docs/account?topic=account-groups#access_ag) with the appropriate scope for the {{site.data.keyword.satelliteshort}} resources and IAM platform and service roles for the actions you want to let users in your access group perform.
    * To scope access to the service, use **IBM Cloud Satellite** in the UI or **satellite** in the API or CLI.
    * For help scoping the role to the right {{site.data.keyword.satelliteshort}} resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](#iam-resource-types). You can scope the access policy to the following {{site.data.keyword.satelliteshort}} resources:
      * **Location** in the UI, **location** in the API and CLI.
      * **Configuration** in the UI, **configuration** in the API and CLI.
      * **Link** in the UI, **link** in the API and CLI.
    * For help choosing the right platform and service roles, see the following reference information:
      *   [Platform management roles](#iam-roles-platform)
      *   [Service access roles](#iam-roles-service)
      *   [Common use cases and roles](#iam-roles-usecases)
4.  [Assign the access group](/docs/account?topic=account-groups#access_ag) with the appropriate scope for any other {{site.data.keyword.cloud_notm}} services that you plan to use in your {{site.data.keyword.satelliteshort}} location. Refer to each service documentation for information about the level of access that you need. Common services include:
    * {{site.data.keyword.openshiftlong_notm}} clusters: **Kubernetes Service** in the UI, **containers-kubernetes** in the API and CLI.
    * {{site.data.keyword.registrylong_notm}} for a private registry across clusters: **Container Registry** in the UI, **container-registry** in the API and CLI.
    * {{site.data.keyword.cos_full_notm}} for the backing storage for your location information: **Cloud Object Storage** in the UI, **cos** in the API and CLI.
5.  [Assign the access group](/docs/account?topic=account-groups#access_ag) with the **Viewer** platform access role to any resource groups that you plan to use with {{site.data.keyword.satelliteshort}}.

<br />


### Example UI steps
{: #iam-assign-ui}

1.  Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2.  From the menu bar, click **Manage > Access (IAM)**.
3.  Click **Access groups**, and then click the access group that you want to assign access to {{site.data.keyword.satellitelong_notm}}.
4.  Click the **Access policies** tab, and then click **Assign access**.
5.  With the **IAM Services** tile selected, in the service access dropdown field, select **IBM Cloud Satellite**. 
    
    You can start to enter letters like `sat` and the field filters results to help you find **IBM Cloud Satellite**.
    {: tip}

6.  Leave the setting in **Account** so that you can scope the resource to a specific instance.
7.  For **Resource Type** string equals field, select **Location**.
8.  For the **Resource** string equals field, enter the name of your {{site.data.keyword.satelliteshort}} location, such as **Port-NewYork**.
9.  For **Platform access**, select the **Editor** role so that all users in your access group can add and remove hosts and endpoints from the {{site.data.keyword.satelliteshort}} location, but cannot create or delete locations. For other roles by resource type, see [IAM platform and service roles](#iam-roles).
10. Click **Add+**.
11. In the **Access summary** pane, review the access policy, and then click **Assign**.
12. From the access group **Access policies** table, verify that the Editor policy is added to the access group.

<br />


### Example CLI steps
{: #iam-assign-cli}

1.  Log in to {{site.data.keyword.cloud_notm}}. If you have a federated account, include the `--sso` flag.
    ```
    ibmcloud login [--sso]
    ```
    {: pre}
2.  Create an {{site.data.keyword.cloud_notm}} IAM access policy for {{site.data.keyword.satellitelong_notm}}. Scope the access policy based on what you want to assign access to. For more information, review the following example commands and table.

    Example command to assign a user the Administrator role for all your {{site.data.keyword.satelliteshort}} locations in the default resource group:
    ```
    ibmcloud iam user-policy-create user@email.com --service-name satellite --resource-group-name default --resource-type location --roles Administrator
    ```
    {: pre}

    Example command to assign an access group the Editor role to a specific {{site.data.keyword.satelliteshort}} location:
    ```
    ibmcloud iam access-group-policy-create team1 --service-name satellite --resource-type location --resource Port-NewYork --roles Editor
    ```
    {: pre}

    <table summary="The table describes the access areas that you can scope the policy to by using CLI flags. Rows are to be read from the left to right, with the scope in column one, the CLI flag in column two, and the description in column three.">
    <caption>Options to scope the access policy</caption>
      <thead>
      <th>Scope</th>
      <th>CLI flag</th>
      <th>Description</th>
      </thead>
      <tbody>
      <tr>
      <td>User</td>
      <td>N/A</td>
      <td>You can assign the policy to an individual or group of users. Place this positional argument immediately following the command.
      <ul><li>**Individual user**: Enter the email address of the user.</li>
      <li>**Access group**: Enter the name of the access group of users. You can create an access group with the `ibmcloud iam access-group-create` command. To list available access groups, run `ibmcloud iam access-groups`. To add a user to an access group, run `ibmcloud iam access-group-user-add <access_group_name> <user_email>`.</li></ul></td>
      </tr>
      <tr>
      <td>{{site.data.keyword.cloud_notm}} service</td>
      <td>`--service-name`</td>
      <td>Enter `satellite` to scope the access policy to {{site.data.keyword.satellitelong_notm}}.</td>
      </tr>
      <tr>
      <td>Resource group</td>
      <td>`--resource-group-name`</td>
      <td>You can grant a policy for a resource group. If you do not specify a resource group, the policy applies to all service instances for all resource groups. To list available resource groups, run `ibmcloud resource groups`.</td>
      </tr>
      <tr>
      <td>{{site.data.keyword.satelliteshort}} resource</td>
      <td>`--resource-type`</td>
      <td>You can limit the policy to a type of resource within {{site.data.keyword.satellitelong_notm}}, such as all {{site.data.keyword.satelliteshort}} locations or {{site.data.keyword.satelliteshort}} configurations. To review resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](#iam-resource-types). Possible values include `location` `configuration`, `cluster`, `clustergroup`, `subscription`, and `resource`.</td>
      </tr>
      <tr>
      <td>Resource instance</td>
      <td>`--resource`</td>
      <td>If you scope the policy to a resource type, you can further limit the policy to a particular instance of the resource. To list available instances, run [the CLI commands](/docs/satellite?topic=satellite-satellite-cli-reference) for that resource type. For example, to list available locations, run `ibmcloud sat location ls`. </td>
      </tr>
      <tr>
      <td>Role</td>
      <td>`--role`</td>
      <td>Choose the platform management or service access that you want to assign.
      <ul><li>**Platform**: Grants access to {{site.data.keyword.satelliteshort}} platform resources so that users can manage infrastructure resources such as locations, hosts, or link endpoints. For more information, see [Platform management roles](#iam-roles-platform). Possible values are: `Administrator`, `Operator`, `Editor`, or `Viewer`.</li>
      <li>**Service**: Grants access to services that run within {{site.data.keyword.satelliteshort}} resources so that users can work with {{site.data.keyword.satelliteshort}} Config subscriptions and Kubernetes resources. For more information, see [Service access roles](#iam-roles-service). Possible values are: `Manager`, `Writer`, or `Reader`.</li></ul></td>
      </tr>
      </tbody>
      </table>
    
3.  Verify that the user or access group has the assigned role.
    *   For individual users:
        ```
        ibmcloud iam user-policies <user@email.com>
        ```
        {: pre}
    *   For access groups:
        ```
        ibmcloud iam access-group-policies <access_group>
        ```
        {: pre}



## IAM platform and service roles
{: #iam-roles}

Every user that accesses the {{site.data.keyword.satelliteshort}} service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.
{: shortdesc}

The name for the {{site.data.keyword.satellitelong}} service in IAM is:
* **IBM Cloud Satellite** in the UI
* **satellite** in the API and CLI

Policies enable access at different levels. Some of the options include the following:

* Access across all {{site.data.keyword.satelliteshort}} service instances of all resource types in your account.
* Access to specific resource types within {{site.data.keyword.satelliteshort}}. For more information about resource types, see [Understanding {{site.data.keyword.satelliteshort}} resource types for access](#iam-resource-types).
  * **Location** in the UI, **location** in the API and CLI.
  * {{site.data.keyword.satelliteshort}} Config resource types:
    * **Cluster** in the UI, **cluster** in the API and CLI.
    * **Clustergroup** in the UI, **clustergroup** in the API and CLI.
    * **Configuration** in the UI, **configuration** in the API and CLI.
    * **Resource** in the UI, **resource** in the API and CLI.
    * **Subscription** in the UI, **subscription** in the API and CLI.
  
* Access to an individual resource of a particular resource type, such as a particular location {{site.data.keyword.satelliteshort}}.

After you define the scope of the access policy, you assign a role, which determines the user's level of access. Review the following sections that outline what actions each platform and service role allows within the {{site.data.keyword.satelliteshort}} service.

### Platform management roles
{: #iam-roles-platform}

Click the tabs in the following table to review the actions that are mapped to platform management roles for different components of {{site.data.keyword.satelliteshort}}. Platform management roles enable users to perform tasks on service resources at the platform level, for example, assign user access for the service, create or delete instances, and bind instances to applications.
{: shortdesc}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Create a {{site.data.keyword.satelliteshort}} location | `/createController` | [`location create`](/docs/satellite?topic=satellite-satellite-cli-reference#location-create) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| List {{site.data.keyword.satelliteshort}} locations | `/getControllers` | [`location ls`](/docs/satellite?topic=satellite-satellite-cli-reference#location-ls) | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Get the details of a {{site.data.keyword.satelliteshort}} location | `/getController` | [`location get`](/docs/satellite?topic=satellite-satellite-cli-reference#location-get) | |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |  
| Remove a {{site.data.keyword.satelliteshort}} location | `/removeController` | [`location rm`](/docs/satellite?topic=satellite-satellite-cli-reference#location-rm) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Attach a host to a {{site.data.keyword.satelliteshort}} location | `/hostqueue/createRegistrationScript` | [`host attach`](/docs/satellite?topic=satellite-satellite-cli-reference#host-attach) | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Assign a host to a {{site.data.keyword.satelliteshort}} location control plane or cluster | `/hostqueue/createAssignment`| [`host assign`](/docs/satellite?topic=satellite-satellite-cli-reference#host-assign) | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| List hosts in a {{site.data.keyword.satelliteshort}} location | `/hostqueue/getHosts` | [`host ls`](/docs/satellite?topic=satellite-satellite-cli-reference#host-ls) | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Update a host | `/hostqueue/updateHost` | [`host update`](/docs/satellite?topic=satellite-satellite-cli-reference#host-update) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Remove a host from a {{site.data.keyword.satelliteshort}} location control plane or cluster| `/hostqueue/removeHost` |[`host rm`](/docs/satellite?topic=satellite-satellite-cli-reference#host-rm) | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Create an {{site.data.keyword.openshiftshort}} cluster in {{sie.data.keyword.satelliteshort}} | `/createCluster` | [`ibmcloud ks cluster create satellite`](/docs/openshift?topic=openshift-kubernetes-service-cli#cli_cluster-create-satellite) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| List {{site.data.keyword.satelliteshort}} clusters | `/getClusters` | [`ibmcloud sat cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls) | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
{: row-headers}
{: #table1}
{: tab-title="Locations and hosts"}
{: class="comparison-tab-table"}
{: tab-group="iam-platform"}
{: caption="Actions that you can take with platform management roles" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the action in the first column, the API for the action in the second column, the CLI for the action in the third column, and the different platform roles in the following columns: none, viewer, editor, operator, and administrator."}


| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Create a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `addChannel`|[`config create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-create) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Delete a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config |`removeChannel` |[`config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rm) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| View an organization in {{site.data.keyword.satelliteshort}} Config | `organization`| - | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| View Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `resources` calls | `resource ls`| | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
{: row-headers}
{: #table3}
{: tab-title="Configuration"}
{: class="comparison-tab-table"}
{: tab-group="iam-platform"}
{: caption="Actions that you can take with platform management roles" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the action in the first column, the API for the action in the second column, the CLI for the action in the third column, and the different platform roles in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Set the configuration version that a cluster group is subscribed to in {{site.data.keyword.satelliteshort}} Config | `setSubscription`| `subscription update --version`| | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
{: row-headers}
{: #table4}
{: tab-title="Subscription"}
{: class="comparison-tab-table"}
{: tab-group="iam-platform"}
{: caption="Actions that you can take with platform management roles" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the action in the first column, the API for the action in the second column, the CLI for the action in the third column, and the different platform roles in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Attach a cluster to {{site.data.keyword.satelliteshort}} Config | `registerCluster` | [`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register)| | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Get a script to register a cluster to {{site.data.keyword.satelliteshort}} Config | `enableRegistrationUrl`|[`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Remove a cluster from {{site.data.keyword.satelliteshort}} Config | `deleteCluster`|[`cluster unregister`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-unregister) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| View clusters that are attached to {{site.data.keyword.satelliteshort}} Config |`clusters` calls |[`cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls)</br> [`cluster get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-get) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
{: row-headers}
{: #table5}
{: tab-title="Cluster"}
{: class="comparison-tab-table"}
{: tab-group="iam-platform"}
{: caption="Actions that you can take with platform management roles" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the action in the first column, the API for the action in the second column, the CLI for the action in the third column, and the different platform roles in the following columns: none, viewer, editor, operator, and administrator."}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Add clusters to a cluster group|`groupClusters`|[`cluster-group attach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach)| | | | |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Remove clusters from a cluster group|`unGroupClusters`|[`cluster-group detach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-detach)| | | | |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|Create a cluster group|`addGroup`|[`cluster-group create`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-create)| | | | |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| View cluster groups that are attached to {{site.data.keyword.satelliteshort}} Config |`groups` |[`cluster-group ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-ls) </br>[`cluster-group get`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-get) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Remove a cluster group |`removeGroup` |[`cluster-group rm`](https://test.cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-rm) | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
{: row-headers}
{: #table6}
{: tab-title="Clustergroup"}
{: class="comparison-tab-table"}
{: tab-group="iam-platform"}
{: caption="Actions that you can take with platform management roles" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the action in the first column, the API for the action in the second column, the CLI for the action in the third column, and the different platform roles in the following columns: none, viewer, editor, operator, and administrator."}


### Service access roles
{: #iam-roles-service}

Click the tabs in the following table to review the actions that are mapped to service access roles for different components of {{site.data.keyword.satelliteshort}}. Service access roles enable users access to {{site.data.keyword.satelliteshort}} and the ability to call the {{site.data.keyword.satelliteshort}} APIs.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Config uses a [custom IAM service access role](/docs/account?topic=account-custom-roles), **Deployer**, in addition to the standard **Reader**, **Writer**, and **Manager** roles. You can assign users the **Deployer** role so that they can deploy existing configurations to your clusters, but cannot add or edit the actual configurations for your apps.
{: note}



| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|
| View clusters that are attached to {{site.data.keyword.satelliteshort}} Config | `clusters` calls | `cluster ls` | |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| View cluster groups that are attached to {{site.data.keyword.satelliteshort}} Config | `groups` | `cluster-group ls` | |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| View a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `channel` | `config get`| |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| View a Kubernetes resource that is managed by {{site.data.keyword.satelliteshort}} Config | `resource` | `resource get`| |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | |  |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| View an organization in {{site.data.keyword.satelliteshort}} Config |`organization` | - | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Create a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `addChannel`|`config create`| | |  || <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Update a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `editChannel`| `config rename`| | |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Add a version to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `addChannelVersion`| `config version create`| | |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |  |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Delete a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `removeChannel` |`config rm`| | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| View a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `subscription`| `subscription get`| |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Create a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `addSubscription`| `subscription create`| | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Update a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `editSubscription`| `subscription update`| | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Set the configuration version of Kubernetes resources for a subscription in {{site.data.keyword.satelliteshort}} Config | `setSubscription`|`subscription update --version` | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Delete a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config |`removeSubscription` |`subscription rm` | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
| Attach a cluster to {{site.data.keyword.satelliteshort}} Config | `registerCluster` | `cluster register` | | | |  |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Get a script to register a cluster to {{site.data.keyword.satelliteshort}} Config | `enableRegistrationUrl`|`cluster register` | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Remove a cluster from {{site.data.keyword.satelliteshort}} Config | `deleteCluster`|`cluster rm` | | | |  |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| View clusters that are attached to {{site.data.keyword.satelliteshort}} Config | `clusters` calls |`cluster ls` | | | |  |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| View cluster groups that are attached to {{site.data.keyword.satelliteshort}} Config | `groups` |`cluster-group ls` | | | |  |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Manage cluster groups that are attached to {{site.data.keyword.satelliteshort}} Config | `assign, unassign, editClusterGroup`<br>`group, ungroupClusters`<br>`add, removeGroup` |`cluster-group attach`, `cluster-group detach`, `cluster-group edit` | | | |  |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| View an organization in {{site.data.keyword.satelliteshort}} Config | `organization`| - | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Manage an organization in {{site.data.keyword.satelliteshort}} Config | | | | | |  |<img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Set the configuration version that a cluster group is subscribed to in {{site.data.keyword.satelliteshort}} Config | `setSubscription`| `subscription update --version` | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| View Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config | `resources` calls | `resource ls` | | | | | <img src="images/icon-confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
{: row-headers}
{: caption="Actions that you can take with service access roles" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the action in the first column, the API for the action in the second column, the CLI for the action in the third column, and the different service roles in the following columns: none, reader, writer, and manager."}

### Platform and service roles for {{site.data.keyword.openshiftshort}} clusters
{: #iam-roles-clusters}

If you create {{site.data.keyword.openshiftlong_notm}} clusters to use in your {{site.data.keyword.openshiftshort}} locations, you manage access to these clusters in IAM for the {{site.data.keyword.openshiftshort}} service, not for {{site.data.keyword.openshiftshort}}. Review the following information to manage IAM access to {{site.data.keyword.openshiftshort}} clusters.
{: shortdesc}

* [Reference documentation](/docs/openshift?topic=openshift-access_reference) for user access permissions including [platform](/docs/openshift?topic=openshift-access_reference#iam_platform) and [service](/docs/openshift?topic=openshift-access_reference#service) roles.
* [Assigning access to clusters](/docs/openshift?topic=openshift-users), such as setting up the API key for underlying infrastructure permissions and granting users access with {{site.data.keyword.cloud_notm}} IAM.
* [Accessing clusters](/docs/openshift?topic=openshift-access_cluster) on the public or private service endpoints, or by using an {{site.data.keyword.cloud_notm}} IAM API key such as for automation purposes.

### Common use cases and roles
{: #iam-roles-usecases}

Wondering which access roles to assign to your {{site.data.keyword.satelliteshort}} access groups and users? Use the examples in following table to determine which roles and scope to assign.
{: shortdesc}

| Use case | Example roles and scope |
| --- | --- |
| Location auditor | **Viewer** platform role for the {{site.data.keyword.satelliteshort}} location and link endpoints. **Reader** service role for the configuration resources in the location. |
| App developers | **Viewer** platform role for the {{site.data.keyword.satelliteshort}} location. **Writer** or **Deployer** service access role for the configuration resources. **Editor** platform role and **Writer** service role to {{site.data.keyword.openshiftshort}} clusters or particular projects in a cluster.|
| Billing | **Viewer** platform role for all the {{site.data.keyword.satelliteshort}} locations in the account. |
| Location administrator | **Administrator** platform role for the location and link resources. **Administrator** platform role to {{site.data.keyword.openshiftshort}} clusters.|
| DevOps operator | **Editor** platform role for the location and link resources. **Deployer** service role for the configurations. **Operator** platform role to {{site.data.keyword.openshiftshort}} clusters.|
| Operator or site reliability engineer | **Administrator** platform role for the location and link resources. **Manager** service role for the configuration resources. **Administrator** platform role and **Manager** service role to {{site.data.keyword.openshiftshort}} clusters. |
{: caption="Types of roles you might assign to meet different use cases." caption-side="top"}
{: summary="The first column contains the use case, which is typically the role of an access group or user. The second column is the example role and scope of the role that you assign the user in {{site.data.keyword.cloud_notm}} IAM."}

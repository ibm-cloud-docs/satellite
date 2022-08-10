---

copyright:
  years: 2020, 2022
lastupdated: "2022-08-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Service access roles
{: #iam-service-access}

Use the following tables to review the actions that are mapped to service access roles for different components of {{site.data.keyword.satelliteshort}}. Service access roles enable users access to {{site.data.keyword.satelliteshort}} and the ability to call the {{site.data.keyword.satelliteshort}} APIs.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Config uses a [custom IAM service access role](/docs/account?topic=account-custom-roles), **Deployer**, in addition to the standard **Reader**, **Writer**, and **Manager** roles. You can assign users the **Deployer** role so that they can deploy existing configurations to your clusters, but cannot add or edit the actual configurations for your apps.
{: note}

## Link service access roles
{: #service-access-link}

| Action | API | CLI | None | Reader | Writer | Manager | {{site.data.keyword.satelliteshort}} Link Administrator | {{site.data.keyword.satelliteshort}} Link Source Access Controller |
|-----|---|---|-----|-----|-----|--------|---|---|
| Export the configuration for a {{site.data.keyword.satelliteshort}} endpoint to a file. | `GET ​/v1​/locations​/{location_id}​/endpoints​/{endpoint_id}​/export`	| | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |	|
| Import the configuration for a {{site.data.keyword.satelliteshort}} endpoint from a file. | `POST /v1/locations/{location_id}/endpoints/import` | | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |	|
| List the {{site.data.keyword.satelliteshort}} endpoints that a client (source) is configured for and the enabled status of the client (source) for each endpoint. | `GET /v1​/locations​/{location_id}​/sources​/{source_id}​/endpoints` | | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |	|
| Enable or disable a client (source) for one or more {{site.data.keyword.satelliteshort}} endpoints.	| `PATCH ​/v1​/locations​/{location_id}​/sources​/{source_id}​/endpoints` | | | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with service access roles." caption-side="bottom"}

## Satelle Config service access roles
{: #service-access-config}

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
{: caption="Actions that you can take with service access roles." caption-side="bottom"}

## Subscription service access roles
{: #service-access-subscription}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| View a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `subscription`| [`subscription get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-get)   \n  [`subscription ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-ls)| |![Feature available.](images/icon-checkmark-filled.svg) | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
| Create a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `addSubscription`| [`subscription create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-create)| | | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
| Update a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `editSubscription`| [`subscription update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update)| | | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
| Set the configuration version of Kubernetes resources for a subscription in {{site.data.keyword.satelliteshort}} Config. | `setSubscription`|[`subscription update --version`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update) | | | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
| Delete a subscription to a configuration of Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. |`removeSubscription` |[`subscription rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-rm) | | | | ![Feature available.](images/icon-checkmark-filled.svg) |![Feature available.](images/icon-checkmark-filled.svg)|
{: caption="Actions that you can take with service access roles." caption-side="bottom"}

## Cluster service access roles
{: #service-access-cluster}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| Attach a cluster to {{site.data.keyword.satelliteshort}} Config. | `registerCluster` | [`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register) | | | |  |![Feature available.](images/icon-checkmark-filled.svg) |
| Get a script to register a cluster to {{site.data.keyword.satelliteshort}} Config. | `enableRegistrationUrl`|[`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a cluster from {{site.data.keyword.satelliteshort}} Config. | `deleteCluster`|[`cluster unregister`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-unregister) | | | |  |![Feature available.](images/icon-checkmark-filled.svg) |
| View clusters that are attached to {{site.data.keyword.satelliteshort}} Config. | `clusters` calls |[`cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls)   \n  [`cluster get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-get) | |![Feature available.](images/icon-checkmark-filled.svg) | |  |![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with service access roles." caption-side="bottom"}

## Cluster group service access roles
{: #service-access-cluster-grp}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| Add clusters to a cluster group.|`groupClusters`|[`cluster-group attach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
| Remove clusters from a cluster group.|`unGroupClusters`|[`cluster-group detach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-detach)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
|Create a cluster group.|`addGroup`|[`cluster-group create`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-create)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
| View cluster groups that are attached to {{site.data.keyword.satelliteshort}} Config. |`groups` |[`cluster-group ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-ls)   \n [`cluster-group get`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-get) | | ![Feature available.](images/icon-checkmark-filled.svg)| | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a cluster group. |`removeGroup` |[`cluster-group rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-rm) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with service access roles." caption-side="bottom"}

## Resources service access roles
{: #service-access-cluster}

| Action | API | CLI | None | Reader | Writer | Deployer | Manager |
|-----|---|---|-----|-----|-----|--------|----|
| View Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `resources` calls | [`resource ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-ls)   \n  [`resource get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-get) | | ![Feature available.](images/icon-checkmark-filled.svg) | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with service access roles." caption-side="bottom"}

`*` You cannot scope access policies to a particular {{site.data.keyword.satelliteshort}} Config **resource**. Instead, scope the policy to the {{site.data.keyword.satellitelong_notm}} service so that users can list {{site.data.keyword.satelliteshort}} Config resources.
{: note}




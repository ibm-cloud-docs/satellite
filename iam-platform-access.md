---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud, platform access for satellite, satellite iam access, platform access roles for satellite

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Platform access roles
{: #iam-platform-access}

Platform access roles enable users to perform tasks on service resources at the platform level. For example, you can assign user access for the service, create or delete instances, and bind instances to applications. Review the following tables for the actions that are mapped to platform access roles for different components of {{site.data.keyword.satelliteshort}}. 
{: shortdesc}

## Location and host platform access roles
{: #platform-location-host}

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
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}

## Link platform access roles
{: #platform-link}

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
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}

## Satellite Config platform access roles
{: #platform-config}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Create a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `addChannel`|[`config create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-create) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Delete a configuration for Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. |`removeChannel` |[`config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rm) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| View an organization in {{site.data.keyword.satelliteshort}} Config. | `organization`| - | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}

## Subscription platform access roles
{: #platform-subscription}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Set the configuration version that a cluster group is subscribed to in {{site.data.keyword.satelliteshort}} Config. | `setSubscription`| [`subscription update --version`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update)| | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}

## Cluster platform access roles
{: #platform-cluster}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Attach a cluster to {{site.data.keyword.satelliteshort}} Config. | `registerCluster` | [`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register)| | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Get a script to register a cluster to {{site.data.keyword.satelliteshort}} Config. | `enableRegistrationUrl`|[`cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a cluster from {{site.data.keyword.satelliteshort}} Config. | `deleteCluster`|[`cluster unregister`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-unregister) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| View clusters that are attached to {{site.data.keyword.satelliteshort}} Config. |`clusters` calls |[`cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls)  \n  [`cluster get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-get) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}

## Cluster group platform access roles
{: #platform-cluster-group}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| Add clusters to a cluster group.|`groupClusters`|[`cluster-group attach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
| Remove clusters from a cluster group.|`unGroupClusters`|[`cluster-group detach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-detach)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
|Create a cluster group.|`addGroup`|[`cluster-group create`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-create)| | | | |![Feature available.](images/icon-checkmark-filled.svg)|
| View cluster groups that are attached to {{site.data.keyword.satelliteshort}} Config. |`groups` |[`cluster-group ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-ls)   \n [`cluster-group get`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-get) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
| Remove a cluster group. |`removeGroup` |[`cluster-group rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-rm) | | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}

## Resource platform access roles
{: #platform-resource}

| Action | API | CLI | None | Viewer | Editor | Operator | Administrator |
|-----|---|---|-----|-----|-----|--------|---|
| View Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config. | `resources` calls | `resource ls`| | | | | ![Feature available.](images/icon-checkmark-filled.svg) |
{: caption="Actions that you can take with platform access roles." caption-side="bottom"}


`*` You cannot scope access policies to a particular {{site.data.keyword.satelliteshort}} Config **resource**. Instead, scope the policy to the {{site.data.keyword.satellitelong_notm}} service so that users can list {{site.data.keyword.satelliteshort}} Config resources.
{: note}

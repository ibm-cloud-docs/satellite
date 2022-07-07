---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite, satellite at events, satellite activity tracker, satconfig events, satlink events, events for satellite config, events for satellite link, events for satellite location, events for satellite host

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Auditing events for {{site.data.keyword.satelliteshort}}
{: #at_events}

As a security officer, auditor, or manager, you can use {{site.data.keyword.at_full}} to track how users and applications interact with {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started#getting-started). 



## Events for {{site.data.keyword.satelliteshort}} clusters
{: #at_actions_clusters}

See the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-at_events).
{: shortdesc}

## Events for the {{site.data.keyword.satelliteshort}} Link component
{: #at_actions_link}

| Action             | Description      |
|--------------------|------------------|
| `satellite.link.create` | A {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.satelliteshort}} Link connector are created. |
| `satellite.link.delete` | A {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.satelliteshort}} Link connector are removed. |
| `satellite.link.get` | Details for a {{site.data.keyword.satelliteshort}} location are retrieved. |
| `satellite.link-endpoint-certs.delete` | A TLS certificate for a {{site.data.keyword.satelliteshort}} endpoint is removed.|
| `satellite.link-endpoint-certs.get` | A list of TLS certificates that are used for a {{site.data.keyword.satelliteshort}} endpoint is retrieved. |
| `satellite.link-endpoint-certs.upload` | A TLS certificate is uploaded for a {{site.data.keyword.satelliteshort}} endpoint.|
| `satellite.link-endpoints.create` | A {{site.data.keyword.satelliteshort}} endpoint is created.  |
| `satellite.link-endpoints.delete` | A {{site.data.keyword.satelliteshort}} endpoint is removed. |
| `satellite.link-endpoints.export`	| The configuration for a {{site.data.keyword.satelliteshort}} endpoint is exported to a file. |
| `satellite.link-endpoints.get` | Details for a {{site.data.keyword.satelliteshort}} endpoint are retrieved. |
| `satellite.link-endpoints.import` |	The configuration for a {{site.data.keyword.satelliteshort}} endpoint is imported from a file. |
| `satellite.link-endpoints.list` | A list of {{site.data.keyword.satelliteshort}} endpoints is retrieved. |
| `satellite.link-endpoints.update` | A {{site.data.keyword.satelliteshort}} endpoint is updated or data for the endpoint is exported. |
| `satellite.link-source-endpoints.list` | A list of {{site.data.keyword.satelliteshort}} endpoints that a client (source) is configured for and the enabled status of the client (source) for each endpoint is retrieved. |
| `satellite.link-source-endpoints.update` | A client (source) is enabled or disabled for one or more {{site.data.keyword.satelliteshort}} endpoints.	|
| `satellite.link-sources.create` | A client (source) is configured for a {{site.data.keyword.satelliteshort}} endpoint. |
| `satellite.link-sources.delete` | A client (source) is removed from a {{site.data.keyword.satelliteshort}} endpoint. |
| `satellite.link-sources.list` | A list of clients (sources) that are configured for a {{site.data.keyword.satelliteshort}} endpoint is retrieved. |
| `satellite.link-sources.update` | A client (source) configuration is updated for a {{site.data.keyword.satelliteshort}} endpoint. |
{: caption="Satellite Link actions that generate events." caption-side="top"}

## Events for the {{site.data.keyword.satelliteshort}} Config component
{: #at_actions_config}

| Action             | Description      |
|--------------------|------------------|
| `satellite.config-cluster.register` | A {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} cluster is registered with {{site.data.keyword.satelliteshort}} Config.|
| `satellite.config-cluster.update` | A {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} cluster is updated with {{site.data.keyword.satelliteshort}} Config.|
| `satellite.config-cluster.detach` | A {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} cluster is detached from {{site.data.keyword.satelliteshort}} Config and can no longer receive configuration.|
| `satellite.config-clusters.detach` | One or more {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} clusters are detached from {{site.data.keyword.satelliteshort}} Config and can no longer receive configuration.|
| `satellite.config-clusters.group` | One or more {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} clusters are added to a cluster group. |
| `satellite.config-clusters.ungroup` | One or more {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} clusters are removed from a cluster group. |
| `satellite.config-clustergroups.edit` | A {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} cluster sets the cluster group or cluster groups of which it is a member. |
| `satellite.config-clustergroups.assign` | One or more {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} clusters are added to one or more cluster groups. |
| `satellite.config-clustergroups.unassign:` | One or more {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} clusters are removed from one or more cluster groups. |
| `satellite.config-configuration.create` | A {{site.data.keyword.satelliteshort}} configuration is created.|
| `satellite.config-configuration.update` | A {{site.data.keyword.satelliteshort}} configuration is updated.|
| `satellite.config-configuration.delete` | A {{site.data.keyword.satelliteshort}} configuration is removed. |
| `satellite.config-configuration.addversion` | A version is added to a {{site.data.keyword.satelliteshort}} configuration. |
| `satellite.config-configuration.removeversion` | A version is removed from a {{site.data.keyword.satelliteshort}} configuration. |
| `satellite.config-subscription.create` | A {{site.data.keyword.satelliteshort}} subscription is created.|
| `satellite.config-subscription.update` | A {{site.data.keyword.satelliteshort}} subscription is updated.|
| `satellite.config-subscription.delete` | A {{site.data.keyword.satelliteshort}} subscription is removed.|
| `satellite.config-subscription.setversion` | A {{site.data.keyword.satelliteshort}} subscription is set to a specific configuration version. |
| `satellite.config-group.create` | A cluster group is created. |
| `satellite.config-group.delete` | A cluster group is removed. |
{: caption="Satellite Config actions that generate events." caption-side="top"}


## Events for {{site.data.keyword.satelliteshort}} storage
{: #at_actions_storage}

| Action | Description |
| --- | --- |
| `satellite.storage-template.get` | A storage template is retrieved. |
| `satellite.storage-template.list` | A list of storage templates is retrieved. | 
| `satellite.storage-configuration.get` | A storage configuration is retrieved. | 
| `satellite.storage-configuration.list` | A list of storage configurations is retrieved. | 
| `satellite.storage-configuration.get-desired` | A desired storage configuration is returned. |
| `satellite.storage-configuration.set-desired` | A desired storage configuration request is created. |
| `satellite.storage-configuration.delete-desired` | A storage request is deleted. |
| `satellite.storage-configuration.expand-desired` | The desired capacity of a storage request is increased. |
| `satellite.storage-configuration.ack-desired-capacity` | A storage capacity expansion request is acknowledged. |
| `satellite.storage-configuration.get-assigned` | Assigned storage configuration details is returned. | 
| `satellite.storage-configuration.create` | A storage configuration is created. | 
| `satellite.storage-configuration.delete` | A storage configuration is deleted. | 
| `satellite.storage-configuration.update` | A storage configuration is updated. |
{: caption="Satellite storage actions that generate events." caption-side="top"}


## Events for {{site.data.keyword.satelliteshort}} assignments
{: #at_actions_assignments}

| Action | Description |
| --- | --- |
| `satellite.subscription.get` | The details of assignment are retrieved. |
| `satellite.subscription.list` | A list of assignments is retrieved. | 
| `satellite.subscription.get-by-name` | An assignment is retrieved. |
| `satellite.subscription.get-by-config` | An assignment is retrieved by using the associated configuration. | 
| `satellite.subscription.get-by-controller` | A list of assignments is retrieved by using a location. |
| `satellite.subscription.get-by-clusterid` | A list of assignments is retrieved by using the cluster ID. | 
| `satellite.subscription.create` | An assignment is created. | 
| `satellite.subscription.delete` | An assignment is deleted. | 
| `satellite.subscription.update` | An assignment is updated. | 
| `satellite.available.classes.get` | A list of available storage classes for an assignment is retrieved. |
{: caption="Assignment actions that generate events." caption-side="top"}



## Viewing events for {{site.data.keyword.satelliteshort}}
{: #at_ui}

Events that are generated by {{site.data.keyword.satellitelong_notm}} are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance in the {{site.data.keyword.cloud_notm}} region that the {{site.data.keyword.satelliteshort}} location location is managed from. {{site.data.keyword.at_full_notm}} has only one instance per region, which you can use to view events for the {{site.data.keyword.cloud_notm}} services in that region. For more information, see [Navigating to the UI](/docs/activity-tracker?topic=activity-tracker-launch).
{: shortdesc}


{{site.data.keyword.iamlong}} (IAM) events are available in the **Frankfurt (eu-de)** region. To view these events, you must provision an instance of the {{site.data.keyword.at_full_notm}} service in the **Frankfurt (eu-de)** region. Then navigate to the {{site.data.keyword.at_full_notm}} UI.




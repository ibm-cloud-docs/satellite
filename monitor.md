---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Monitoring for {{site.data.keyword.satelliteshort}}
{: #monitor}

{{site.data.keyword.satellitelong}} comes with basic tools to help you manage the health of your {{site.data.keyword.satelliteshort}} resources, such as reviewing location and host health. Additionally, you can integrate {{site.data.keyword.satelliteshort}} and other {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.mon_full}} to get a comprehensive view and tools to manage all your resources.
{: shortdesc}

Monitoring for your {{site.data.keyword.satelliteshort}} location and for the {{site.data.keyword.cloud_notm}} services that run in your location must be set up separately. For example, to collect metrics for your {{site.data.keyword.satelliteshort}} location setup, you enable a {{site.data.keyword.mon_short}} instance to collect platform metrics in the same region that your location is managed from. Then, to collect metrics for a {{site.data.keyword.openshiftlong_notm}} cluster that runs in your {{site.data.keyword.satelliteshort}} location, you create a monitoring agent in your cluster to automatically collect and forward pod metrics to a {{site.data.keyword.mon_short}} instance. Note that you can use the same {{site.data.keyword.mon_short}} instance to collect metrics for both your {{site.data.keyword.satelliteshort}} location and services that run in your {{site.data.keyword.satelliteshort}} location.

## Understanding what is logged and monitored by default
{: #health-default}

By default, {{site.data.keyword.satelliteshort}} generates certain activity events and monitors the state of your location and host resources.
{: shortdesc}

### Auditing events for {{site.data.keyword.satelliteshort}} actions
{: #audit-events}

See [Auditing events for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-at_events).
{: shortdesc}

### {{site.data.keyword.IBM_notm}} monitoring to resolve and report location alerts
{: #monitoring-default}

When you create a {{site.data.keyword.satelliteshort}} location and set up the location control plane, {{site.data.keyword.IBM_notm}} automatically monitors and resolves certain alerts for issues with your location setup and host infrastructure. The following table describes different scenarios and the actions that {{site.data.keyword.IBM_notm}} takes to address the scenarios.
{: shortdesc}

Additionally, if you [set up your {{site.data.keyword.satelliteshort}} location to forward logs to {{site.data.keyword.la_full_notm}}](/docs/satellite?topic=satellite-health#setup-la), the messages and more detailed information from the {{site.data.keyword.IBM_notm}} Monitoring component are captured and stored in your {{site.data.keyword.la_full_notm}} instance.

| Scenario | Action |
| --- | --- |
| Location control plane does not have a host in three separate zones. | Check for attached, unassigned hosts in the location. If a host is available, assign the host to the location control plane for the missing zone, giving preference to hosts with a label that matches the zone. |
| Cluster capacity exceeds 80% in a zone. | Prevent or allow {{site.data.keyword.redhat_openshift_notm}} clusters to be created. Assign available hosts to a location control plane for more compute resources. |
| {{site.data.keyword.redhat_openshift_notm}} clusters are in an unhealthy state. | Resolve certain health issues with {{site.data.keyword.redhat_openshift_notm}} clusters. |
| Default monitoring tools like Prometheus do not work. | Send alerts to your {{site.data.keyword.la_full_notm}} instance and return a status message with further troubleshooting information. |
| Ingress subdomain registration fails. | Alert {{site.data.keyword.IBM_notm}} engineers to troubleshoot the issues further and return a status message with further troubleshooting information. |
{: caption="{{site.data.keyword.IBM_notm}} monitoring actions to address certain scenarios." caption-side="top"}


## Viewing location, host, and cluster health
{: #view-health}

You can review the health of {{site.data.keyword.satelliteshort}} resources such as locations, hosts, clusters, and Kubernetes resources that run in clusters.
{: shortdesc}

### Viewing location health
{: #location-health}

When you set up a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your location healthy. For more information, see [{{site.data.keyword.IBM_notm}} monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-monitor#monitoring-default). For troubleshooting help, see [Debugging location health](/docs/satellite?topic=satellite-ts-locations-debug).
{: shortdesc}

You can review the host health from the **Locations** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, or by running `ibmcloud sat location ls`.

| Health state | Description
| --- | --- |
| `action required` | The location needs your attention. Check the status and message for more information, and try [debugging your location](/docs/satellite?topic=satellite-ts-locations-debug). |
| `completed` | {{site.data.keyword.satelliteshort}} completed the setup of the location control plane components on the hosts that you assigned to the control plane. The location is soon ready to be used.|
| `completing` | {{site.data.keyword.satelliteshort}} is setting up the location control plane components on the hosts that you assigned to the control plane. Check back in a little while.|
| `critical` | The {{site.data.keyword.satelliteshort}} location control plane needs your attention. Check the status and message for more information, and try [debugging your location control plane](/docs/satellite?topic=satellite-ts-locations-control-plane).|
| `failed` | {{site.data.keyword.satelliteshort}} did not successfully resolve issues in your location. For more information, see the status message. |
| `host required` | The {{site.data.keyword.satelliteshort}} location is created, but you must [assign hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane). Assign hosts in multiples of 3, such as 6, 9, or 12. |
| `normal` | The {{site.data.keyword.satelliteshort}} location is ready to use. |
| `provisioning` | The control plane for the {{site.data.keyword.satelliteshort}} is provisioning. You cannot assign hosts to other {{site.data.keyword.satelliteshort}} resources, such as clusters, in the location until the control plane is ready.|
| `resolving` | {{site.data.keyword.satelliteshort}} is trying to resolve issues for you, such as by assigning available hosts to the control plane to relieve capacity issues. For more information, see the status message. |
{: caption="Location health states." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the location. The second column describes what the health state means."}

### Viewing host health
{: #host-health}

When you attach hosts to a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your hosts healthy. For more information, see [{{site.data.keyword.IBM_notm}} monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-monitor#monitoring-default). For troubleshooting help, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).
{: shortdesc}

You can review the host health from the **Hosts** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, or by running `ibmcloud sat host ls --location <location_name_or_ID>`.

| Health state | Description |
| --- | --- |
| `assigned` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster. View the status for more information. If the status is `-`, the hosts did not complete the bootstrapping process to the {{site.data.keyword.satelliteshort}} resource. For hosts that you just assigned, wait an hour or so for the process to complete. If you still see the status, [log in to the host to continue debugging](/docs/satellite?topic=satellite-ts-hosts-login).|
| `health-pending` | The host is assigned and bootstrapped into the cluster as worker nodes that are provisioned and deployed. However, the health components that {{site.data.keyword.IBM_notm}} sets up in the host cannot communicate status back to {{site.data.keyword.cloud_notm}}. Make sure that your hosts meet the [minimum host and network connectivity requirements](/docs/satellite?topic=satellite-reqs-host-network) and that the hosts are not blocked by a firewall in your infrastructure provider. |
| `provisioning` | The host is attached to the {{site.data.keyword.satelliteshort}} location and is in the process of bootstrapping to become part of a {{site.data.keyword.satelliteshort}} resource, such as the worker node of a {{site.data.keyword.openshiftlong_notm}} cluster. While the host reports a `provisioning` state, the worker node goes through the states of provisioning and deploying. |
| `ready` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).|
| `normal` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster, and ready for usage. |
| `reload-required` | The host is attached to the {{site.data.keyword.satelliteshort}} location, but requires a reload before it can be assigned to a {{site.data.keyword.satelliteshort}} resource. For example, you might have deleted a {{site.data.keyword.satelliteshort}} cluster, and now all the hosts from the cluster require a reload. To reload a host, you must [remove the host from the location](/docs/satellite?topic=satellite-host-remove), reload the operating system in the underlying infrastructure provider, and [attach the host](/docs/satellite?topic=satellite-attach-hosts) back to the location. |
| `unassigned` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual). If you tried to assign the host unsuccessfully, see [Cannot assign hosts to a cluster](/docs/satellite?topic=satellite-assign-fails).|
| `unknown` | The health of the host is unknown. If the host is unassigned, try [assigning the host](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual) to a {{site.data.keyword.satelliteshort}} resource, such as a cluster. If the host is assigned, try debugging the host by following the steps in [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts-debug). If the host still has issues, try removing, updating, and reattaching the host. |
| `unresponsive` | The host did not check in with the {{site.data.keyword.satelliteshort}} location control plane within the past 5 minutes. The host cannot be assigned when it is unresponsive. Try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts-debug), particularly the network connectivity. |
{: caption="Host health states." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the host. The second column describes what the health state means."}

### Viewing cluster health
{: #cluster-health}

To review the health of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-health-monitor#states).
{: shortdesc}

### Viewing Kubernetes resources in clusters
{: #kubernetes-resources-health}

When you add your clusters to {{site.data.keyword.satelliteshort}} Configuration, the Kubernetes resources are automatically added to an inventory that you can review. For more information, see [Deploying Kubernetes resources across clusters with {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-setup-clusters-satconfig).
{: shortdesc}

Adding clusters to {{site.data.keyword.satelliteshort}} Configuration does not automatically set up logging and monitoring solutions, such as {{site.data.keyword.la_full_notm}} and {{site.data.keyword.mon_full_notm}}.
{: note}

### Viewing {{site.data.keyword.satelliteshort}} Config registration status for clusters
{: #satconfig-registration-status}

You can view the registration status of clusters that are enabled for use with {{site.data.keyword.satelliteshort}} Config. Keep in mind that some of these clusters might be in a public cloud location, not your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. List clusters that are registered with {{site.data.keyword.satelliteshort}} Config. Note the output in the **Status** and **Location** columns.
    ```sh
    ibmcloud sat cluster ls
    ```
    {: pre}

2. Review the following {{site.data.keyword.satelliteshort}} Config registration statuses.

| Status | Description |
| --- | --- |
| `active` | {{site.data.keyword.satelliteshort}} Config components for the location are installed in the cluster, and at least one resource is being watched. |
| `inactive` | {{site.data.keyword.satelliteshort}} Config components were manually removed from the cluster, or are installed but are no longer responding to {{site.data.keyword.satelliteshort}} Config. For example, network connectivity might be disconnected. Existing resources, if any, continue to run but do not receive updates. To resolve the issue, try debugging your [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-ts-locations-debug) or [cluster](/docs/openshift?topic=openshift-debug_clusters). |
| `registered` | {{site.data.keyword.satelliteshort}} Config components are installed in the cluster, but no resources are currently watched. To set up Watch-keeper, see [Reviewing resources that are managed by {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-satcon-manage#satconfig-resources). |
{: caption="Host health states." caption-side="top"}



## Setting up {{site.data.keyword.mon_short}} for {{site.data.keyword.satelliteshort}} location platform metrics
{: #setup-mon}

Forward and view additional metrics for {{site.data.keyword.satelliteshort}} in an {{site.data.keyword.mon_full_notm}} instance that is enabled for platform-level metrics.
{: shortdesc}

Metrics are available for the {{site.data.keyword.satelliteshort}} Link component of your location to help you monitor the performance of specific Link endpoints or of all Link endpoints for the location. For example, you can monitor the latency or throughput of a specific Link endpoint that you created.

1. Create or choose an existing {{site.data.keyword.mon_short}} instance.
    - If you already have a {{site.data.keyword.mon_short}} instance in the same {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from, and the {{site.data.keyword.mon_short}} instance is configured to collect platform metrics, the metrics that are generated for your {{site.data.keyword.satelliteshort}} location are automatically forwarded to this {{site.data.keyword.mon_short}} instance.
    - Otherwise, to set up {{site.data.keyword.mon_short}} for your {{site.data.keyword.satelliteshort}} location:
        1. [Provision an {{site.data.keyword.mon_full_notm}} instance](https://cloud.ibm.com/catalog/services){: external} in the same {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from.
        2. [Enable the instance for platform-level metrics collection](/docs/monitoring?topic=monitoring-platform_metrics_enabling). Note that within one region, only one {{site.data.keyword.mon_short}} instance can be enabled for platform metrics collection.
2. In the **Monitoring** dashboard, click **Open Dashboard** for your {{site.data.keyword.mon_short}} instance.
3. In the {{site.data.keyword.mon_short}} dashboard, click **Dashboards** > **{{site.data.keyword.IBM_notm}} ** > **Satellite Link - Overview**. The pre-defined dashboard for {{site.data.keyword.satelliteshort}} Link metrics opens. Note that if you just created this {{site.data.keyword.mon_short}} instance, it might take up to two hours for the **{{site.data.keyword.IBM_notm}} ** dashboards to become available.

    You can create a copy of this dashboard to customize the metrics that are shown. To add metrics that are enabled for {{site.data.keyword.satellitelong_notm}}, search for the `ibm_satellite_link` prefix.
    {: tip}

4. Review the [available metrics](#available-metrics) and [attributes for segmentation](#attributes).
5. Review more ways that you can [work with platform metrics](/docs/monitoring?topic=monitoring-platform_metrics_working).

After you set up {{site.data.keyword.mon_short}} with the pre-defined dashboard for {{site.data.keyword.satelliteshort}} Link metrics, you can quickly access this dashboard from the **Link endpoints** tab of your {{site.data.keyword.satelliteshort}} location console by clicking **Launch monitoring**.

### Available metrics
{: #available-metrics}

Review the following metrics that are available for your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}


#### Location tunnel numbers
{: #ibm_satellite_link_location_tunnel_count}

The total number of {{site.data.keyword.satelliteshort}} Link tunnel servers present at the endpoint.  Three tunnels are created between the tunnel server and the connector to support redundancy across the three availability zones of your location.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_location_tunnel_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID` |
{: caption="Metadata for the location tunnel numbers metric" caption-side="top"}

#### Location latency
{: #ibm_satellite_link_location_rtt_second}

The total round trip time of data in milliseconds for the location.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_location_rtt_second`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID` |
{: caption="Metadata for the location latency metric" caption-side="top"}

#### Location traffic to cloud
{: #ibm_satellite_link_location_to_cloud_data_rate}

The total rate of data in bytes per second in the to-cloud direction for the location.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_location_to_cloud_data_rate`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID` |
{: caption="Metadata for the location traffic to cloud metric" caption-side="top"}

#### Location traffic from cloud
{: #ibm_satellite_link_location_from_cloud_data_rate}

The total rate of data in bytes per second in the from-cloud direction for the location.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_location_from_cloud_data_rate`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID` |
{: caption="Metadata for the location traffic from cloud metric" caption-side="top"}

#### Location traffic total
{: #ibm_satellite_link_location_total_data_rate}

The total rate of data in bytes per second in to-cloud and from-cloud directions for the location.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_location_total_data_rate`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID` |
{: caption="Metadata for the location traffic total metric" caption-side="top"}

#### Endpoint connection count
{: #ibm_satellite_link_endpoint_connection_count}

The total number of connections present at the endpoint.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_endpoint_connection_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID, Endpoint ID, Endpoint Name` |
{: caption="Metadata for the Endpoint connection count metric" caption-side="top"}

#### Endpoint traffic to cloud
{: #ibm_satellite_link_endpoint_to_cloud_data_rate}

The rate of data in bytes per second in the to-cloud direction for the endpoint.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_endpoint_to_cloud_data_rate`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID, Endpoint ID, Endpoint Name` |
{: caption="Metadata for the Endpoint traffic to cloud metric" caption-side="top"}

#### Endpoint traffic from cloud
{: #ibm_satellite_link_endpoint_from_cloud_data_rate}

The rate of data in bytes per second in the from-cloud direction for the endpoint.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_endpoint_from_cloud_data_rate`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID, Endpoint ID, Endpoint Name` |
{: caption="Metadata for the endpoint traffic from cloud metric" caption-side="top"}

#### Endpoint traffic total
{: #ibm_satellite_link_endpoint_total_data_rate}

The total rate of data in bytes per second in to-cloud and from-cloud directions for the endpoint.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_satellite_link_endpoint_total_data_rate`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, Location ID, Endpoint ID, Endpoint Name` |
{: caption="Metadata for the endpoint traffic total metric" caption-side="top"}

### Attributes for segmentation
{: #attributes}

Review the following global and additional attributes that are available for segmentation of {{site.data.keyword.satelliteshort}} metrics.
{: shortdesc}

#### Global attributes
{: #global-attributes}

The following global attributes are available for segmenting all the [available metrics](#available-metrics).
{: shortdesc}

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud Type` | `ibm_ctype` | The cloud type, which can be public, dedicated, or local |
| `Location` | `ibm_location` | The location of the monitored resource, which can be a region, data center, or global |
| `Resource` | `ibm_resource` | The resource that is measured by the service, usually reported as an identifying name or GUID |
| `Resource Type` | `ibm_resource_type` | The type of the resource that is measured by the service |
| `Resource group` | `ibm_resource_group_name` | The resource group where the {{site.data.keyword.satelliteshort}} location was created |
| `Scope` | `ibm_scope` | The account GUID associated with this metric |
| `Service name` | `ibm_service_name` | The name of the service that generates this metric |
{: caption="Global attributes for metric segmentation" caption-side="top"}

#### Additional attributes
{: #additional-attributes}

The following additional attributes that are specific to {{site.data.keyword.satelliteshort}} are available for segmenting one or more of the [available metrics](#available-metrics). See the `Segment By` field for each metric to determine its available segmentation attributes.
{: shortdesc}

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Endpoint ID` | `ibm_satellite_link_endpoint_id` | The identifier of the endpoint |
| `Endpoint Name` | `ibm_satellite_link_endpoint_name` | The name of the endpoint |
| `Location ID` | `ibm_satellite_link_location_id` | The identifier of the location |
| `Service instance` | `ibm_service_instance` | The service instance segment identifies the instance the metric is associated with |
| `Service instance name` | `ibm_service_instance_name` | The user-provided name of the service instance, which might not be unique across regions in the account |
{: caption="Additional attributes for metric segmentation" caption-side="top"}


## Setting up monitoring for clusters
{: #setup-clusters-monitoring}

You cannot currently use the {{site.data.keyword.openshiftlong_notm}} console or the observability plug-in CLI (`ibmcloud ob`) to enable monitoring for {{site.data.keyword.satelliteshort}} clusters. You must manually deploy monitoring agents to your cluster to forward metrics to {{site.data.keyword.mon_short}}.
{: note}

To set up monitoring for {{site.data.keyword.redhat_openshift_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see  [Deploying a monitoring agent in a {{site.data.keyword.redhat_openshift_notm}} cluster](/docs/monitoring?topic=monitoring-config_agent#config_agent_kube_os).





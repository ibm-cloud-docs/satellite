---

copyright:
  years: 2020, 2020
lastupdated: "2020-07-30"

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


# Logging and monitoring {{site.data.keyword.satelliteshort}} health
{: #health}

{{site.data.keyword.satellitelong}} comes with basic tools to help you manage the health of your {{site.data.keyword.satelliteshort}} resources, such as reviewing location and host health.
{: shortdesc}

## Understanding what is logged and monitored by default
{: #health-default}

By default, {{site.data.keyword.satelliteshort}} generates certain activity events and monitors the state of your location and host resources.
{: shortdesc}

### Auditing events for {{site.data.keyword.satelliteshort}} actions
{: #audit-events}

See [Auditing events for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-at_events).
{: shortdesc}

### IBM monitoring to resolve and report location alerts
{: #monitoring-default}

When you create a {{site.data.keyword.satelliteshort}} location and set up the location control plane, IBM automatically monitors and resolves certain alerts for issues with your location setup and host infrastructure. The following table describes different scenarios and the actions that IBM takes to address the scenarios.
{: shortdesc}

| Scenario | Action |
| --- | --- |
| Location control plane does not have a host in three separate zones. | Check for attached, unassigned hosts in the location. If a host is available, assign the host to the location control plane for the missing zone, giving preference to hosts with a label that matches the zone. |
| Cluster capacity exceeds 80% in a zone. | Prevent or allow {{site.data.keyword.openshiftshort}} clusters to be created. Assign available hosts to a location control plane for more compute resources. |
| {{site.data.keyword.openshiftshort}} clusters are in an unhealthy state. | Resolve certain healthy issues with {{site.data.keyword.openshiftshort}} clusters. |
| Default monitoring tools like Prometheus do not work. | Send alerts to your {{site.data.keyword.la_full_notm}} instance and return a status message with further troubleshooting information. |
| Ingress subdomain registration fails. | Alert IBM engineers to troubleshoot the issues further and return a status message with further troubleshooting information. |
| Other alerts | Alert IBM engineers to troubleshoot the issues further and return a status message. |
{: caption="IBM monitoring actions to address certain scenarios." caption-side="top"}
{: summary="Read this table from left to right. In the first column is the scenario. In the second column is the action that {{site.data.keyword.satelliteshort}} automatically takes to address the alert."}

<br />


## Viewing location, host, and cluster health
{: #view-health}

You can review the health of {{site.data.keyword.satelliteshort}} resources such as locations, hosts, clusters, and Kubernetes resources that run in clusters.
{: shortdesc}

### Viewing location health
{: #location-health}

When you set up a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your location healthy. For more information, see [IBM monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-service-architecture#monitoring-default). For troubleshooting help, see [Debugging location health](/docs/satellite?topic=satellite-ts-locations).
{: shortdesc}

You can review the host health from the **Locations** table in the [{{site.data.keyword.satelliteshort}} console](https://test.cloud.ibm.com/satellite/){: external}.

| Health state | Description
| --- | --- |
| `action required` | The location needs your attention. Check the status and message for more information, and try [debugging your location](/docs/satellite?topic=satellite-ts-locations). |
| `completed` | {{site.data.keyword.satelliteshort}} completed the setup of the location control plane components on the hosts that you assigned to the control plane. The location is soon ready to use.|
| `completing` | {{site.data.keyword.satelliteshort}} is setting up the location control plane components on the hosts that you assigned to the control plane. Check back in a little while.|
| `critical` | The {{site.data.keyword.satelliteshort}} location control plane needs your attention. Check the status and message for more information, and try [debugging your location control plane](/docs/satellite?topic=satellite-ts-locations#ts-locations-control-plane).|
| `failed` | {{site.data.keyword.satelliteshort}} did not successfully resolve issues in your location. For more information, see the status message. |
| `host required` | The {{site.data.keyword.satelliteshort}} location is created, but you must [add at least 3 hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).|
| `normal` | The {{site.data.keyword.satelliteshort}} location is ready to use. |
| `provisioning` | The control plane for the {{site.data.keyword.satelliteshort}} is provisioning. You cannot assign hosts to other {{site.data.keyword.satelliteshort}} resources, such as clusters, in the location until the control plane is ready.|
| `resolving` | {{site.data.keyword.satelliteshort}} is trying to resolve issues for you, such as by assigning available hosts to the control plane to relieve capacity issues. For more information, see the status message. |
{: caption="Location health states" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the location. The second column describes what the health state means."}

### Viewing host health
{: #host-health}

When you add hosts to a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your hosts healthy. For more information, see [IBM monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-service-architecture#monitoring-default). For troubleshooting help, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts).
{: shortdesc}

You can review the host health from the **Hosts** table in the [{{site.data.keyword.satelliteshort}} console](https://test.cloud.ibm.com/satellite/){: external}.

| Health state | Description
| --- | --- |
| `assigned` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster. View the status for more information. If the status is `-`, the hosts did not complete the bootstrapping process to the {{site.data.keyword.satelliteshort}} resource. For hosts that you just assigned, wait an hour or so for the process to complete. If you still see the status, [log in to the host to continue debugging](/docs/satellite?topic=satellite-ts-hosts#ts-hosts-login).| 
| `provisioning` | The host is attached to the {{site.data.keyword.satelliteshort}} location and is in the process of bootstrapping to become part of a {{site.data.keyword.satelliteshort}} resource, such as the worker node of a {{site.data.keyword.openshiftlong_notm}} cluster.|
| `ready` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-hosts#host-assign).|
| `reload-required` | The host is attached to the {{site.data.keyword.satelliteshort}} location, but requires a reload before it can be assigned to assigned to a {{site.data.keyword.satelliteshort}} resource. For example, you might have deleted a {{site.data.keyword.satelliteshort}} cluster, and now all of the hosts from the cluster require a reload. To reload a host, you must [remove the host from the location](/docs/satellite?topic=satellite-hosts#host-remove), reload the operating system in the underlying infrastructure provider, and [add the host](/docs/satellite?topic=satellite-hosts#add-hosts) back to the location. |
| `unassigned` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-hosts#host-assign). If you tried to assign the host unsuccessfully, see [Cannot assign hosts to a cluster](/docs/satellite?topic=satellite-ts-hosts#assign-fails).|
| `unknown` | The health of the host is unknown. If the host is unassigned, try [assigning the host](/docs/satellite?topic=satellite-hosts#host-assign) to a {{site.data.keyword.satelliteshort}} resource, such as a cluster. If the host is assigned, try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts). |
| `unresponsive` | The host did not check in with the {{site.data.keyword.satelliteshort}} location control plane within the past 5 minutes. The host cannot be assigned when it is unresponsive. Try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts), particularly the network connectivity. |
{: caption="Host health states" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the host. The second column describes what the health state means."}

### Viewing cluster health
{: #cluster-health}

To review the health of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-health#states_cluster).
{: shortdesc}

### Viewing Kubernetes resources in clusters
{: #kubernetes-resources-health}

When you add your clusters to {{site.data.keyword.satelliteshort}} Configuration, the Kubernetes resources are automatically added to an inventory that you can review.
{: shortdesc}

Adding clusters to {{site.data.keyword.satelliteshort}} Configuration does not automatically set up logging and monitoring solutions, such as {{site.data.keyword.loganalysislong_notm}} and {{site.data.keyword.mon_full_notm}}.
{: note}

---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Logging and monitoring {{site.data.keyword.satelliteshort}} health
{: #health}

Integrate {{site.data.keyword.satelliteshort}} and other {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.la_full}} and {{site.data.keyword.mon_full}} to get a comprehensive view and tools to manage all your resources.
{: shortdesc}

By default, the following logs are automatically generated for your {{site.data.keyword.satelliteshort}} location:
* Messages and more detailed information from the IBM Monitoring component regarding certain alerts for issues with your location setup and host infrastructure
* Health check status of the {{site.data.keyword.satelliteshort}} link tunnel server endpoint

Logging for your {{site.data.keyword.satelliteshort}} location and for the {{site.data.keyword.cloud_notm}} services that run in your location must be set up separately. For example, to collect logs for your {{site.data.keyword.satelliteshort}} location setup, you enable a {{site.data.keyword.la_short}} instance to collect platform logs in the same region that your location is managed from. Then, to collect logs for a {{site.data.keyword.openshiftlong_notm}} cluster that runs in your {{site.data.keyword.satelliteshort}} location, you create a LogDNA agent in your cluster to automatically collect and forward pod logs to a {{site.data.keyword.la_short}} instance. Note that you can use the same LogDNA instance to collect logs for both your {{site.data.keyword.satelliteshort}} location and services that run in your {{site.data.keyword.satelliteshort}} location.

## IBM monitoring to resolve and report location alerts
{: #monitoring-default}

When you create a {{site.data.keyword.satelliteshort}} location and set up the location control plane, IBM automatically monitors and resolves certain alerts for issues with your location setup and host infrastructure. The following table describes different scenarios and the actions that IBM takes to address the scenarios.
{: shortdesc}

Additionally, if you [set up your {{site.data.keyword.satelliteshort}} location to forward logs to {{site.data.keyword.la_full_notm}}](/docs/satellite?topic=satellite-health#setup-logdna), the messages and more detailed information from the IBM Monitoring component are captured and stored in your {{site.data.keyword.la_full_notm}} instance.

| Scenario | Action |
| --- | --- |
| Location control plane does not have a host in three separate zones. | Check for attached, unassigned hosts in the location. If a host is available, assign the host to the location control plane for the missing zone, giving preference to hosts with a label that matches the zone. |
| Cluster capacity exceeds 80% in a zone. | Prevent or allow {{site.data.keyword.openshiftshort}} clusters to be created. Assign available hosts to a location control plane for more compute resources. |
| {{site.data.keyword.openshiftshort}} clusters are in an unhealthy state. | Resolve certain health issues with {{site.data.keyword.openshiftshort}} clusters. |
| Default monitoring tools like Prometheus do not work. | Send alerts to your {{site.data.keyword.la_full_notm}} instance and return a status message with further troubleshooting information. |
| Ingress subdomain registration fails. | Alert IBM engineers to troubleshoot the issues further and return a status message with further troubleshooting information. |
{: caption="IBM monitoring actions to address certain scenarios." caption-side="top"}
{: summary="Read this table from left to right. In the first column is the scenario. In the second column is the action that {{site.data.keyword.satelliteshort}} automatically takes to address the alert."}

<br />

## Setting up LogDNA for {{site.data.keyword.satelliteshort}} location platform logs
{: #setup-logdna}

Forward and view logs that are automatically generated for your {{site.data.keyword.satelliteshort}} location setup in an {{site.data.keyword.la_full_notm}} instance that is enabled for platform-level logs.
{: shortdesc}

If you already have a {{site.data.keyword.la_short}} instance in the same {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from, and the LogDNA instance is configured to collect platform logs, the logs that are generated for your {{site.data.keyword.satelliteshort}} location are automatically forwarded to this LogDNA instance. If you do not, first contact your account administrator to verify that a {{site.data.keyword.la_short}} instance with platform logs does not exist in the region. Otherwise, follow these steps to set up LogDNA for your {{site.data.keyword.satelliteshort}} location.

1. [Provision an {{site.data.keyword.la_full_notm}} instance](https://cloud.ibm.com/catalog/services/ibm-log-analysis-with-logdna){: external} in the same {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from.
2. [Enable the instance for platform-level log collection](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-config_svc_logs). Note that within one region, only one {{site.data.keyword.la_short}} instance can be enabled for platform logs collection.
3. In the **Logging** dashboard, click **View LogDNA** for your {{site.data.keyword.la_short}} instance.
4. In the LogDNA dashboard, review `satellite` logs for your location, which you can identify by searching for your location's ID.
5. Review how you can [search and filter logs in the LogDNA dashboard](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-view_logs).

<br />



## Setting up logging for clusters
{: #setup-clusters}

To understand and set up logging for {{site.data.keyword.openshiftshort}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see the tutorials in the [{{site.data.keyword.la_full_notm}} documentation](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-kube#kube).
{: shortdesc}

You cannot currently use the {{site.data.keyword.openshiftlong_notm}} console or the observability plug-in CLI (`ibmcloud ob`) to enable logging for {{site.data.keyword.satelliteshort}} clusters. You must manually deploy LogDNA agents to your cluster to forward logs to {{site.data.keyword.la_short}}.
{: note}

<br />

## Setting up monitoring for clusters
{: #setup-clusters}

To understand and set up monitoring for {{site.data.keyword.openshiftshort}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see the tutorials in the [{{site.data.keyword.mon_full_notm}} documentation](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-kubernetes_cluster#kubernetes_cluster).
{: shortdesc}

You cannot currently use the {{site.data.keyword.openshiftlong_notm}} console or the observability plug-in CLI (`ibmcloud ob`) to enable monitoring for {{site.data.keyword.satelliteshort}} clusters. You must manually deploy Sysdig agents to your cluster to forward metrics to {{site.data.keyword.mon_short}}.
{: note}

<br />

## Viewing location, host, and cluster health
{: #view-health}

You can review the health of {{site.data.keyword.satelliteshort}} resources such as locations, hosts, clusters, and Kubernetes resources that run in clusters.
{: shortdesc}

### Viewing location health
{: #location-health}

When you set up a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your location healthy. For more information, see [IBM monitoring to resolve and report location alerts](#monitoring-default). For troubleshooting help, see [Debugging location health](/docs/satellite?topic=satellite-ts-locations-debug).
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

When you attach hosts to a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your hosts healthy. For more information, see [IBM monitoring to resolve and report location alerts](#monitoring-default). For troubleshooting help, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).
{: shortdesc}

You can review the host health from the **Hosts** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, or by running `ibmcloud sat host ls --location <location_name_or_ID>`.

| Health state | Description
| --- | --- |
| `assigned` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster. View the status for more information. If the status is `-`, the hosts did not complete the bootstrapping process to the {{site.data.keyword.satelliteshort}} resource. For hosts that you just assigned, wait an hour or so for the process to complete. If you still see the status, [log in to the host to continue debugging](/docs/satellite?topic=satellite-ts-hosts-login).| 
| `provisioning` | The host is attached to the {{site.data.keyword.satelliteshort}} location and is in the process of bootstrapping to become part of a {{site.data.keyword.satelliteshort}} resource, such as the worker node of a {{site.data.keyword.openshiftlong_notm}} cluster.|
| `ready` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-hosts#host-assign).|
| `normal` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster, and ready for usage. |
| `reload-required` | The host is attached to the {{site.data.keyword.satelliteshort}} location, but requires a reload before it can be assigned to a {{site.data.keyword.satelliteshort}} resource. For example, you might have deleted a {{site.data.keyword.satelliteshort}} cluster, and now all of the hosts from the cluster require a reload. To reload a host, you must [remove the host from the location](/docs/satellite?topic=satellite-hosts#host-remove), reload the operating system in the underlying infrastructure provider, and [attach the host](/docs/satellite?topic=satellite-hosts#attach-hosts) back to the location. |
| `unassigned` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-hosts#host-assign). If you tried to assign the host unsuccessfully, see [Cannot assign hosts to a cluster](/docs/satellite?topic=satellite-assign-fails).|
| `unknown` | The health of the host is unknown. If the host is unassigned, try [assigning the host](/docs/satellite?topic=satellite-hosts#host-assign) to a {{site.data.keyword.satelliteshort}} resource, such as a cluster. If the host is assigned, try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts-debug). |
| `unresponsive` | The host did not check in with the {{site.data.keyword.satelliteshort}} location control plane within the past 5 minutes. The host cannot be assigned when it is unresponsive. Try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts-debug), particularly the network connectivity. |
{: caption="Host health states." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the host. The second column describes what the health state means."}

### Viewing cluster health
{: #cluster-health}

To review the health of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-health-monitor#states).
{: shortdesc}

### Viewing Kubernetes resources in clusters
{: #kubernetes-resources-health}

When you add your clusters to {{site.data.keyword.satelliteshort}} Configuration, the Kubernetes resources are automatically added to an inventory that you can review. For more information, see [Deploying Kubernetes resources across clusters with {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config).
{: shortdesc}

Adding clusters to {{site.data.keyword.satelliteshort}} Configuration does not automatically set up logging and monitoring solutions, such as {{site.data.keyword.la_full_notm}} and {{site.data.keyword.mon_full_notm}}.
{: note}



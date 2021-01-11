---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-11"

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

{{site.data.keyword.satellitelong}} comes with basic tools to help you manage the health of your {{site.data.keyword.satelliteshort}} resources, such as reviewing location and host health.
{: shortdesc}


## IBM monitoring to resolve and report location alerts
{: #monitoring-default}

When you create a {{site.data.keyword.satelliteshort}} location and set up the location control plane, IBM automatically monitors and resolves certain alerts for issues with your location setup and host infrastructure. The following table describes different scenarios and the actions that IBM takes to address the scenarios.
{: shortdesc}

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

## Viewing location, host, and cluster health
{: #view-health}

You can review the health of {{site.data.keyword.satelliteshort}} resources such as locations, hosts, clusters, and Kubernetes resources that run in clusters.
{: shortdesc}

### Viewing location health
{: #location-health}

When you set up a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your location healthy. For more information, see [IBM monitoring to resolve and report location alerts](#monitoring-default). For troubleshooting help, see [Debugging location health](/docs/satellite?topic=satellite-ts-locations-debug).
{: shortdesc}

You can review the host health from the **Locations** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external}, or by running `ibmcloud sat location ls`.

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

You can review the host health from the **Hosts** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external}, or by running `ibmcloud sat host ls --location <location_name_or_ID>`.

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

To review the health of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-health#states_cluster).
{: shortdesc}

### Viewing Kubernetes resources in clusters
{: #kubernetes-resources-health}

When you add your clusters to {{site.data.keyword.satelliteshort}} Configuration, the Kubernetes resources are automatically added to an inventory that you can review. For more information, see [Deploying Kubernetes resources across clusters with {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config).
{: shortdesc}

---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-04"

keywords: satellite architecture, satellite components, satellite workload isolation, satellite tenant isolation, satellite dependencies

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


# Learning about {{site.data.keyword.satelliteshort}} architecture, workload isolation, and dependencies
{: #service-architecture}

Learn about the {{site.data.keyword.satellitelong_notm}} service architecture, its components, how your workloads are isolated from other tenants, and what other {{site.data.keyword.cloud_notm}} and third-party services {{site.data.keyword.satelliteshort}} depends on.
{: shortdesc}

## {{site.data.keyword.satelliteshort}} architecture
{: #architecture}

The following image shows the main components in {{site.data.keyword.satellitelong_notm}} and how they interact with each other.
{: shortdesc}

![{{site.data.keyword.satellitelong_notm}} service architecture](/images/sat_architecture.png)

|Master components|Description|
|-----------------|-----------------------|
|{{site.data.keyword.satelliteshort}} Link tunnel|The {{site.data.keyword.satelliteshort}} Link tunnel creates a secure TLS connection to the {{site.data.keyword.satelliteshort}} Link connector that runs on the control plane worker nodes in your {{site.data.keyword.satelliteshort}} location. All communication that leaves and enters your location is proxied by the Link tunnel server, and is captured and monitored by IBM to detect malicious activity. To control the network connections between the workloads that run in your location and the {{site.data.keyword.cloud_notm}} region that manages your location, you can set up {{site.data.keyword.satelliteshort}} Link endpoints.  |
|IBM Monitoring|The IBM Monitoring component monitors the compute capacity in your {{site.data.keyword.satelliteshort}} location and the components that run in your {{site.data.keyword.satelliteshort}} control plane to detect issues and automatically resolve them if possible. These actions can include assigning additional hosts to the control plane or restart components that keep on failing. For issues that cannot be resolved, IBM Site Reliability Engineers are automatically informed for further investigation. |
|{{site.data.keyword.satelliteshort}} Config|With {{site.data.keyword.satelliteshort}} Config, you can consistently deploy Kubernetes resource configurations across {{site.data.keyword.openshiftlong}} clusters that run on the infrastructure of your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}. You can monitor the health of these resources by using the location dashboard.|
{: summary="Overview of the {{site.data.keyword.satelliteshort}} control plane master components"}
{: class="simple-tab-table"}
{: caption="Overview of {{site.data.keyword.satelliteshort}} control plane master components" caption-side="top"}
{: #master-components}
{: tab-title="Master components"}
{: tab-group="satellite-components"}

|Worker node components|Description|
|-----------------|-----------------------|
|{{site.data.keyword.satelliteshort}} Link connector|The {{site.data.keyword.satelliteshort}} link connector component is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. All workloads that run in your location and that must connect to an app, service or server that runs in {{site.data.keyword.cloud_notm}} must send their requests to the {{site.data.keyword.satelliteshort}} Link connector. The {{site.data.keyword.satelliteshort}} Link connector securely forwards your request to the {{site.data.keyword.satelliteshort}} Link tunnel server where the request is proxied and forwarded to the desired target. To enable DNS resolution between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}, you must create a {{site.data.keyword.satelliteshort}} Link endpoint. By default, all network traffic that enters and leaves your location is automatically captured to allow network monitoring and auditing. |
|IBM Monitoring|The IBM Monitoring component monitors the compute capacity in your {{site.data.keyword.satelliteshort}} location and the components that run in your {{site.data.keyword.satelliteshort}} control plane to detect issues and automatically resolve them if possible. These actions can include assigning additional hosts to the control plane or restart components that keep on failing. For issues that cannot be resolved, IBM Site Reliability Engineers are automatically informed for further investigation. |
|Cluster master|When you create a {{site.data.keyword.satelliteshort}} cluster in your location, the master of this cluster is deployed onto your {{site.data.keyword.satelliteshort}} control plane worker nodes to allow communication to {{site.data.keyword.cloud_notm}} and monitoring through IBM. For more information, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters).|
{: summary="Overview of the {{site.data.keyword.satelliteshort}} control plane worker node components"}
{: class="simple-tab-table"}
{: caption="Overview of {{site.data.keyword.satelliteshort}} control plane worker node components" caption-side="top"}
{: #worker-node-components}
{: tab-title="Worker node components"}
{: tab-group="satellite-components"}

<br />


## Dependencies to other {{site.data.keyword.cloud_notm}} services
{: #cloud-service-dependencies}

Review the {{site.data.keyword.cloud_notm}} services that {{site.data.keyword.satellitelong_notm}} connects to over the public network.
{: shortdesc}

|Service name|Description|
|------------|-------------------------------------|
| Business Support Services for {{site.data.keyword.cloud_notm}} (BSS) | The `BSS` component is used to access information about the {{site.data.keyword.cloud_notm}} account, service subscription, service usage, and billing. |
|{{site.data.keyword.cloudcerts_short}}|This service is used to retrieve the TLS certificates for custom domains that {{site.data.keyword.satellitelong_notm}} users set up.|
| Global Search and Tagging (Ghost) | The `Ghost` component is used to look up information about other {{site.data.keyword.cloud_notm}} services, such as IDs, tags, or service attributes. |
| Hypersync and hyperwarp | This {{site.data.keyword.cloud_notm}} component is used to provide information about {{site.data.keyword.satelliteshort}} locations so that the location is visible to other {{site.data.keyword.cloud_notm}} services and location information can be searched and displayed. |
|{{site.data.keyword.cloud_notm}} Command Line (CLI)|When you use the CLI plug-in to perform {{site.data.keyword.satellitelong_notm}} operations, the {{site.data.keyword.satelliteshort}} plug-in connects to the {{site.data.keyword.cloud_notm}} CLI service over the public service endpoint.|
|{{site.data.keyword.registrylong_notm}}|This service is used to store the container images that {{site.data.keyword.satellitelong_notm}} uses to run the service.|
|{{site.data.keyword.la_full_notm}}|{{site.data.keyword.satellitelong_notm}} sends location logs to {{site.data.keyword.la_full_notm}}. These logs are monitored and analyzed by the service team to detect service issues and malicious activities. |
|{{site.data.keyword.databases-for-mongodb_full_notm}}|All data that is sent to the {{site.data.keyword.satelliteshort}} Link or {{site.data.keyword.satellitelong_notm}} Config API is stored in a {{site.data.keyword.databases-for-mongodb}} service instance. Access to this instance is protected by IAM policies and available to the {{site.data.keyword.satelliteshort}} service team only to detect service issues and malicious activity.|
|{{site.data.keyword.mon_full_notm}}|{{site.data.keyword.satellitelong_notm}} sends service metrics to {{site.data.keyword.mon_full_notm}}. These metrics are monitored by the service team to identify capacity and performance issues of the service. |
| {{site.data.keyword.cloudaccesstraillong_notm}} | {{site.data.keyword.satellitelong_notm}} integrates with {{site.data.keyword.at_full_notm}} to forward location audit events to the {{site.data.keyword.at_full_notm}} service instance that is set up and owned by you.|
| {{site.data.keyword.cloud_notm}} Service Endpoint (CSE)|This service is used to connect to the private service endpoints of other {{site.data.keyword.cloud_notm}} services and to set up a private service endpoint for the {{site.data.keyword.satelliteshort}} Link component. |
| IBMid profile service | The IBMid component is used to look up the IBMid from an email address. The IBMid is used to authenticate with {{site.data.keyword.cloud_notm}} via Identity and Access Management (IAM). |
| IBM QRadar Log Manager|To enable monitoring of the network traffic that flows between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}, the {{site.data.keyword.satelliteshort}} Link component uses IBM QRadar Log Manager. The log manager creates networking events, analyzes and correlates these events to identify compliance risks, anomalies, potential attacks and insider threats.  |
| Identity and Access Management (IAM) | To authenticate requests to the service and authorize user actions, {{site.data.keyword.satellitelong_notm}} implements platform and service access roles in Identity and Access Management (IAM).  |
|{{site.data.keyword.cos_short}} (COS)|This service is used to back up the control plane data of a {{site.data.keyword.satelliteshort}} location. The service instance is owned by the customer who controls access to the instance by using IAM policies. All data is encrypted in transit and at rest.|

<br />


## Dependencies to 3rd party services
{: #3rd-party-dependencies}

Review the list of third-party services that {{site.data.keyword.satellitelong}} connects to over the public network.
{: shortdesc}

|Service name|Description|
|------------|-------------------------------------|
|Akamai, Cloudflare|Akamai and Cloudflare are used as the primary providers for DNS, global load balancing, and web firewall capabilities in {{site.data.keyword.satellitelong_notm}}.|
|Amplitude, Segment|Amplitude and Segment are used to monitor user behavior in the {{site.data.keyword.cloud_notm}} dashboard, such as page hits or click-through paths. This information is used for IBM-internal marketing and data analytics purposes.|
|Launch Darkly|To manage the roll out of new features in {{site.data.keyword.satellitelong_notm}}, Launch Darkly feature flags are used. A feature flag controls the visibility and availability of a feature to a selected user base.|
|Let's Encrypt|This service is used as the Certificate authority to generate SSL certificates for customer owned public endpoints. All generated certificates are managed in {{site.data.keyword.cloudcerts_long_notm}}.|
|Slack|Slack is used as the IBM-internal communication medium to troubleshoot issues and bring together internal SMEs to resolve customer issues. Diagnostic information about a {{site.data.keyword.satelliteshort}} location are sent to a private Slack channel and include the customer account ID, location ID, and details about the {{site.data.keyword.satelliteshort}} control plane.|
